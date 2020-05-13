# Workspace structure with Rust
A simple example on how to use workspace with Rust.  

## Creating the project structure  
The idea here is to create the folder/crates structure below.
```
+ my-project  [lib type]
| - main-one  [main type] 
| - main-two  [main type]
| - shared    [lib type]
```
... and to do this just run the following commands:
```
cargo new my-project --lib
cd my-project
cargo new main-one
cargo new main-two
cargo new shared --lib
```  

## Configuring our root Cargo.toml file
Now we have to tell ```cargo``` that our main crate will work as a workspace.  
To do that, just open the `Cargo.toml` file of `my-project` crate and replace eveything with the code below.  
```
[workspace]
members = [
    "main-one",
    "main-two",
    "shared",
]
```
At this point you can build your project already and check if all crates are compiled with success.
```
> cargo build
   Compiling main-two v0.1.0 (D:\path-to-project\my-project\main-two)
   Compiling shared v0.1.0 (D:\path-to-project\my-project\shared)
   Compiling main-one v0.1.0 (D:\path-to-project\my-project\main-one)
```

## Using code from shared crate on main projects  
Open the `Cargo.toml` of crate `main-one` and add the `shared` crate as a depedency to it.
```
[dependencies]
shared = { path = "../shared" }
```
That is it :)
Check line 2 of `main.rs` of crate `main-one` if you want to see how you can use actual code of `shared` crate.

## How to access code from shared crate but from another file instead of lib.rs
**1.** Create a module file, for example, `models.rs`.  
**2.** Add it on the `lib.rs` and don't forget to make it public, like so `pub mod models`.  
**3.** On the consumer file (`main.rs`) just use `shared::models::name-of-file-or-function-or-whatever`

You can also check `main.rs` of crate `main-one` for examples.

## How to access files from shared crate but my module is inside a folder
**1.** Create a folder with your module name, for example, `systems`.  
**2.** Create your sub-module files like `system_a.rs` and `system_b.rs`.  
**3.** Inside the module folder, create a file called `mod.rs` and expose your sub-modules, like so  
```
pub mod system_a;
pub mod system_b;
```
**4.** Expose your module in `lib.rs` and don't forget to make it public `pub mod systems`.  
**5.** On the consumer file (`main.rs`) just use `shared::systems::system_a::{}`

Again, you can also check `main.rs` of crate `main-one` for examples.

## Recommended reading
[About workspace and members](https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html)  
[About filestruct](https://doc.rust-lang.org/stable/rust-by-example/mod.html)  
[Mods and "submods"](https://doc.rust-lang.org/stable/rust-by-example/mod/split.html)  

## Disclaimer
I'm complitly new to Rust and comming from .NET C# is being a real struggle for me. So I may not be using the correct terms by referring to cargo, rust, crates and expose and even doing something that is potentially wrong. If this is the case, feel free to suggest corrections.