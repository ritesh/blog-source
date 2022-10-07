+++
categories = [""]
tags = ["webassembly", "rust", "newbie"]
title = "Adventures Wasm: Part One"
date = "2022-10-06T16:22:48+01:00"
draft = false
+++

# Adventures in WebAssembly Land 

*This is Part 1 of my notes on trying out WASM for a non-trivial task. Stay tuned for more fun.*

I've been on the hype-train again, and the flavour of the month (year? decade?) is Web Assembly. During breaks between the job that I'm paid to do, I like to keep up to date on new things and sharpen my technical skills to keep up with the clever people on my team. In this post you can follow my adventures in learning about an unfamiliar tech stack. 

Web Assembly, in a nutshell, allows you to use your favourite programming language to write code that can target web-browsers by compiling it into a WASM blob. Your code in Go/Rust/Python goes in -> *magic compilation step** -> a WASM binary comes out.  The compilation step converts your code into instructions for a stack-machine, which can be run either in your browser or in a non-browser context. 

If you're curious as to what it looks like when compiled for the web here's an excerpt from a WASM file. Looks a bit like x86 assembly or decompiled Java, no?

```javascript
 global.get $global0
    local.set $var6
    i32.const 2464
    local.set $var7
    local.get $var6
    local.get $var7
    i32.sub
    local.set $var8
    local.get $var8
    global.set $global0
    local.get $var8
    local.get $var0
    i32.store offset=2380
    local.get $var8
    local.get $var1
    i32.store offset=2384
    local.get $var8
    local.get $var2
    i32.store offset=2388
    local.get $var8
    local.get $var3
    i32.store offset=2392
    local.get $var8
    local.get $var4
    i32.store offset=2396
    i32.const 216
    local.set $var9
    local.get $var8
    local.get $var9
    i32.add
    local.set $var10
    local.get $var10
    local.get $var3
    local.get $var4
    call $func2957
```

WASM opens up a whole new world of possibilities, limited only by the constraints of the stack-machine that runs WebAssembly code. Most of these constraints are security related (e.g. disallowing direct file access), which make sense since with WASM you're running a blob of inscrutable code that could potentially harm your computer. 

The most obvious use cases that come to mind are to implement code that is performance sensitive in your favourite performant programming language and then compiling that to WASM. You would then have a JavaScript shim to call that code. If you like Rust, [this is a good place to learn how it all fits together](https://rustwasm.github.io/docs/book/game-of-life/introduction.html)

The reference implementation for WASM is written in Rust so writing code in Rust seemed like a natural place to start. To learn more about how interop works between WASM and JavaScript, I decided (foolishly) to try and implement a WebAuthn client in WASM. You can do this quite easily in [pure JavaScript/TypeScript](https://webauthn.guide/) - but where's the fun in that? WebAuthentication (and Apple's PassKeys) allow you to use a security key or another authenticator like TouchID or FaceID in the place of a password to log-in to websites.

*Side Note: You can learn more about Web Authentication [on this slick website](https://webauthn.io/).*


## Bootstrapping
 I've given myself a problem, where do I start? My end goal is to get a WebAuthn flow running but we are only allowed to use Rust to generate the WASM we need. Users should be able to enroll security keys and then subsequently authenticate using these security keys.  We need a project structure that allows us to write Rust code and automagically generate WASM and run it in  a browser so that I can test stuff as I write it.

I decided to go with a Rust framework called [Yew](https://github.com/yewstack/yew) to build this project. Yew seems similar to React in that you create components and then define lifecycle functions (like on_create, update etc). You can send messages to components based on actions in the web UI so we can use things like buttons/text fields *etc.*  to capture user input and send it to rust code. 

There's a nice little starter template that's been created by the Yew folks that you can clone and instantly [start hacking on](https://github.com/yewstack/yew-trunk-minimal-template). Note that this template doesn't make use of `wasm-pack` directly and I'd never heard of `trunk`, [the tool used](https://github.com/yewstack/yew-trunk-minimal-template) to bundle/build the application (similar to what `npm run start` would give you in JS land). Using this template I began hacking on a crude WebAuthn implementation. 

To make life more ~~difficult~~ interesting, I decided to use only terminal based editors (vim/emacs) to write code for this. If this was an actual project, I'd reach for VSCode because it works really well for all kinds of non-trivial projects. 

## Fighting with Types
Rust is a strongly-typed language and forces you to think about the types you will use. JavaScript is not very fussy about this and the Rust representation of the main JavaScript types are [`JsValue`](https://rustwasm.github.io/wasm-bindgen/api/wasm_bindgen/struct.JsValue.html) and [`Object`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html)which can pretty much represent most things things in JavaScript. 

For the majority of the code, I'm struggling with figuring out how to coerce a Rust type into a shape that I can use to call the JavaScript (or more specifically [`web_sys`](https://rustwasm.github.io/wasm-bindgen/web-sys/index.html)) APIs. I am both a terrible Rust and JavaScript programmer :D

For example, the TypeScript/JavaScript code to enrol credentials, via [this lovely post](https://www.imperialviolet.org/2022/09/22/passkeys.html):

``` javascript
var createOptions : CredentialCreationOptions = {
  publicKey: {
    rp: {
      // The RP ID. This needs some thought. See comments below.
      id: SEE_BELOW,
      // This field is required to be set to something, but you can
      // ignore it.
      name: "",
    },

    user: {
      // `userIdBase64` is the user's passkey ID, from the database,
      // base64-encoded.
      id: Uint8Array.from(atob(userIdBase64), c => c.charCodeAt(0)),
      // `username` is the user's username. Whatever they would type
      // when signing in with a password.
      name: username,
      // `displayName` can be a more human name for the user, or
      // just leave it blank.
      displayName: "",
    },

   // .... boiler plate removed
  }
};

//The magic happens here
navigator.credentials.create(createOptions).then(
  handleCreation, handleCreationError);
```

You create a `CredentialCreationOptions`  object which contains parameters that you'd like to use to enrol a new web authenticator. Then you call `navigator.credentials.create(..)` with these parameters to enrol the credentials.

The equivalent using `web_sys` in Rust

```rust
let pk1 = PubKeyAlg {
                r#type: "public-key".into(),
                alg: -7,
            };
            let pk2 = PubKeyAlg {
                r#type: "public-key".into(),
                alg: -257,
            };
            //Using this convoluted way to get from &[u8] to the type that the API expects
            // TODO: Challenge bytes are not meant to be deterministic
            let challenge_bytes = vec![1, 2, 3];
            let challenge = Bytes::new(challenge_bytes.as_slice());
            //overkill!
            //Using this convoluted way to get from &[u8] to the type that the API expects
            let user_id_bytes = vec![255];
            let user_id = Bytes::new(user_id_bytes.as_slice());
            let pubkey_opt = PublicKeyCredentialCreationOptions::new(
                // challenge
                &serde_wasm_bindgen::to_value(challenge).unwrap().into(),
                //pub_key_cred_params:
                &serde_wasm_bindgen::to_value(&vec![&pk1, &pk2]).unwrap(),
                //rp
                &PublicKeyCredentialRpEntity::new("foo"),
                //user:
                &PublicKeyCredentialUserEntity::new(
                    "bar",
                    "ritesh",
                    &serde_wasm_bindgen::to_value(user_id).unwrap().into(),
                ),
            );
            let mut copt = CredentialCreationOptions::new();
            copt.public_key(&pubkey_opt);
            let window = web_sys::window().expect("missing window");
            let navigator = window.navigator();
            if let Ok(p) = navigator.credentials().create_with_options(&copt) {
                //WHy unused?
                // How do we update state?
                let cl = Closure::new(|_| self.records.); //incomplete
                //TODO: Use this promise that is returned
                let _ = p.then(&cl);
                //See: https://github.com/rustwasm/wasm-bindgen/blob/main/examples/closures/src/lib.rs#L72-L82
                // If you don't do this .forget() call, the code creates a DOMException - not sure I fully understand why yet
                cl.forget();
            } else {
                console::log_1(&"booo".into())
            }


```

Ignore the bits about closures for now  - we are creating a  `CredentialCreationOptions` struct which has a `PublicKeyCredentialCreationOptions` struct inside it. Both of these are autogenerated from JavaScript so the API is a little bit clunky. I'm having to jump through weird type conversions to get to the expected parameters required by these functions. Some of it is obvious, like converting a `&str` into a `String` type since `&str` is a borrowed type, and we want our struct to own the String. Some of it is bizarre, and I'll have to figure out a better way. Like going from `Vec<u8>` to a `serde_bytes::Bytes` and then slicing it. 


### Fighting with Closures and Lifetimes
So coming back to `Closures` - why do I need one anyway? What I'm trying to do is implement the key enrolling process in Rust, and to trigger it the user clicks a button. When they click the "Enroll" button (optionally with a username or other data), the Rust code calls `navigator.credentials.create()` and tries to store the auth info in LocalStorage as a record. In a real system this would be a database somewhere. 

When you call `navigator.credentials.create()` it gives you a `Promise`. Usually in JS you would do a `somepromise.then(something else)` the `something else` is a function that would be invoked if the promise resolved successfully. I'm trying to pass a Rust closure as `something else`  so that I can update LocalStorage with the key enrollment. 

Unfortunately, the limitation on the Closure that must be passed in is that its lifetime needs to be `'static` .  Recall that `'static` is the longest possible lifetime in Rust - i.e. when something is annotated with `'static` it is meant to live as long as the program is running. 

If I were to pass in (move?) `self` into the closure, I cannot guarantee that `self` has a `'static
lifetime so it does not work.


### Where next?
So far, I have the bare minimum working. I can take events from JS and pass them to my Rust code so that I can do something useful. Things to do go get this working properly

* A simple form that allows you to enter a username/password
* Figure out how to share state between the rust code and the JavaScript code
* Remove ugly hacks (unlikely)
* Get the flow working and then check it for correctness against a proper WebAuthn/PassKeys flow

Not sure how far I'll make it, but stay tuned!
