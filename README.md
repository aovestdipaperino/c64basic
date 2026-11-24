# c64basic

[![GitHub stars](https://img.shields.io/github/stars/aovestdipaperino/c64basic?logo=github&color=brightgreen)](https://github.com/aovestdipaperino/c64basic/stargazers)

A Commodore 64 BASIC V2 interpreter in Rust that runs original petcat-format
listings and renders the 40x25 PETSCII screen in a modern terminal with
Unicode and ANSI colors. Glyph mappings are verified against the actual C64
character generator ROM, execution is paced to authentic C64 speed, and a
`--c64-font` flag renders with the real C64 charset when the terminal uses
the [C64 Pro Mono](https://style64.org/c64-truetype) font.

Extracted from the [football-manager-ita](https://github.com/aovestdipaperino/football-manager-ita)
preservation project; the bundled game listings serve as test fixtures.

```bash
# Run at authentic C64 speed
cargo run --release -p c64basic -- footballmanager.txt

# Full speed, real C64 glyphs (terminal font must be C64 Pro Mono)
cargo run --release -p c64basic -- --speed max --c64-font footballmanager.txt

# Scripted play: inject keypresses over TCP
cargo run --release -p c64basic -- --keyport 6464 footballmanager.txt
printf 'G' | nc 127.0.0.1 6464
```

Other flags: `--seed <n>` for deterministic runs, `--headless <n>` to run n
statements and dump the screen, `--parse-only` to validate a listing.

## License

MIT. The bundled game listings are the work of their original author,
Daniele Piccoli, included for preservation and testing.

## Browser (WebAssembly)

The interpreter compiles to wasm and runs the international edition in a
canvas, blitting 8x8 glyphs straight from the C64 character generator ROM.

```bash
wasm-pack build c64basic --target web --out-dir ../web/pkg -- --no-default-features
python3 -m http.server -d web 8642   # then open http://localhost:8642
```

The prebuilt `web/` directory is committed, so any static host (GitHub
Pages included) can serve it as-is.
