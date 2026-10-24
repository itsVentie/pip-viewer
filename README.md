# PiP Viewer

Minimalistic, always-on-top Picture-in-Picture (PiP) video viewer built with **Tauri v2**, **Svelte 5**, **TypeScript**, and **Bun**. Designed to quickly embed and float YouTube or custom video streams over all your windows without extra clutter.

## Tech Stack

- **Desktop Framework:** [Tauri v2](https://tauri.app/)
- **Frontend Framework:** [Svelte 5](https://svelte.dev/) + TypeScript
- **Bundler / Dev Server:** [Vite](https://vitejs.dev/)
- **Package Manager / Runtime:** [Bun](https://bun.sh/)

## Prerequisites

Make sure you have the following installed on your system:
- [Rust](https://www.rust-lang.org/) (latest stable)
- [Bun](https://bun.sh/) (latest stable)
- Tauri v2 prerequisites for your OS (e.g., WebView2 on Windows)

## Getting Started

1. Clone the repository and navigate to the project directory.
2. Install dependencies:
   ```bash
   bun install

```

3. Run the development environment:
```bash
bun tauri dev

```



## Build for Production

To build the optimized desktop application bundle for your system:

```bash
bun tauri build

```
