# project path of article [![alt text][1]](https://www.hecatron.com/posts/2024/rust-wasm-debug/)
<!-- keep the format -->
## setup
<!-- keep the format -->
## Install some needed tools
<!-- keep the format -->
```bash <!-- markdownlint-disable-line code-block-style -->
# We need the wasm target for rust
rustup target add wasm32-unknown-unknown
# The trunk utility for building rust to wasm
cargo install trunk
# If we use the nightly toolchain we gain some features from leptos
rustup toolchain install nightly
```
<!-- keep the format -->
<!-- keep the format -->
## Show which toolchain is active
<!-- keep the format -->
```bash <!-- markdownlint-disable-line code-block-style -->
rustup show
# or better
rustup show |sed -n '/active toolchain/,/^$/p'
```
<!-- keep the format -->
<!-- keep the format -->
## Setting up a demo project
<!-- keep the format -->
```bash <!-- markdownlint-disable-line code-block-style -->
# cargo init dummy_project
# cd dummy_project
# Deviating from the article description
# german  - Abweichend von der Artikelbeschreibung
# used sccache for reduce compile time - a compiler wrapper and avoids compilation when possible, storing cached
export RUSTC_WRAPPER=sccache
cargo init .
# used sccache for reduce compile time
export RUSTC_WRAPPER=sccache
cargo add leptos --features=csr,nightly
cargo add console_error_panic_hook console_log log
# switch to nightly rust
rustup override set nightly
# used sccache for reduce compile time - a compiler wrapper and avoids compilation when possible, storing cached
export RUSTC_WRAPPER=sccache
# plain build the project
cargo build
```
<!-- keep the format -->
<!-- keep the format -->
## Create the root index html
<!-- keep the format -->
```bash <!-- markdownlint-disable-line code-block-style -->
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
  <head>
    <link data-trunk rel="rust" data-keep-debug="true" data-wasm-opt="z"/>
    <link data-trunk rel="icon" type="image/ico" href="/public/favicon.ico"/>
  </head>
<body></body>
</html>
EOF
```
<!-- keep the format -->
<!-- keep the format -->
## Edit/replace the main.rs file
<!-- keep the format -->
```bash <!-- markdownlint-disable-line code-block-style -->
cat > ./src/main.rs << 'EOF'
use dummy_project::SimpleCounter;
use leptos::*;

pub fn main() {
    _ = console_log::init_with_level(log::Level::Debug);
    console_error_panic_hook::set_once();
    mount_to_body(|| {
        view! {
            <SimpleCounter
                initial_value=0
                step=1
            />
        }
    })
}
EOF
```
<!-- keep the format -->
## Create a lib.rs file
<!-- keep the format -->
```bash <!-- markdownlint-disable-line code-block-style -->
FILE_NAME="./src/lib.rs"
cat > $FILE_NAME << 'EOF'
use leptos::*;

/// A simple counter component.
///
/// You can use doc comments like this to document your component.
#[component]
pub fn SimpleCounter(
    /// The starting value for the counter
    initial_value: i32,
    /// The change that should be applied each time the button is clicked.
    step: i32,
) -> impl IntoView {
    let (value, set_value) = create_signal(initial_value);

    view! {
        <div>
            <button on:click=move |_| set_value.set(0)>"Clear"</button>
            <button on:click=move |_| set_value.update(|value| *value -= step)>"-1"</button>
            <span>"Value: " {value} "!"</span>
            <button on:click=move |_| {
                // Test Panic
                //panic!("Test Panic");
                // In order to breakpoint the below, the code needs to be on it's own line
                set_value.update(|value| *value += step)
            }
            >"+1"</button>
        </div>
    }
}
EOF
unset FILE_NAME # only the name without dollar sign
echo $?
```
<!-- keep the format -->
## Create a favicon file
<!-- keep the format -->
```bash <!-- markdownlint-disable-line code-block-style -->
mkdir public && touch public/favicon.ico
```
<!-- keep the format -->

<!-- keep the format -->
## Template Dummy code block
<!-- keep the format -->
```bash <!-- markdownlint-disable-line code-block-style -->
# code here
```
<!-- keep the format -->
## Template Dummy generate file
<!-- keep the format -->
```bash <!-- markdownlint-disable-line code-block-style -->
FILE_NAME="/tmp/here_filename.txt"
cat > $FILE_NAME << 'EOF'
echo "Filename" $0
EOF
unset FILE_NAME # only the name without dollar sign
echo $?
```
<!-- keep the format -->
For further steps, see Project path [![alt text][1]](project_path.md)
<!-- make folder and download the link sign vai curl -->
<!-- mkdir -p img && curl --create-dirs --output-dir img -O  "https://raw.githubusercontent.com/MathiasStadler/link_symbol_svg/refs/heads/main/link_symbol.svg"-->
<!-- Link sign - Don't Found a better way :-( - You know a better method? - **send me a email** -->
[1]: ./img/link_symbol.svg
<!-- keep the format -->