# [Main/Original llama.cpp](https://github.com/ggml-org/llama.cpp)

# With

# [ai-janitor llama-metal](https://github.com/ai-janitor/llama-metal)

## Just combination of the latest llama.cpp Code and the Fixed and optimizations by ai-janitor

Upstream base: [`c6f6a92c5`](https://github.com/ggml-org/llama.cpp/commit/c6f6a92c5) (2026-08-14)

### What is changed

Metal backend fixes for discrete (non-Apple-Silicon) GPUs, in `ggml/src/ggml-metal/`:

- Managed storage mode instead of Shared for buffers on non-unified-memory GPUs, so PCIe reads are cached. CPU writes call `didModifyRange:` and CPU reads call `synchronizeResource` first.
- Concurrent dispatch is off by default on non-Apple GPUs. `MTLDispatchTypeConcurrent` has broken memory barriers there.
- `mul_mv_ext` no longer aborts on unsupported `ne11` when the GPU has no simdgroup matmul. It falls back to `r1ptg = 4`.

Env overrides for testing: `GGML_METAL_MANAGED_BUFFERS_ENABLE`, `GGML_METAL_MANAGED_BUFFERS_DISABLE`, `GGML_METAL_CONCURRENCY_DISABLE`.

### Build

```sh
cmake -B build -DGGML_METAL=ON -DCMAKE_BUILD_TYPE=Release
cmake --build build -j
```

Tested on a Mac Pro (2019) with an AMD Radeon PRO W6900X, macOS 26.6.1.

For all upstream docs, see the [main repo](https://github.com/ggml-org/llama.cpp).
