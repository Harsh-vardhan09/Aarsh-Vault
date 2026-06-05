## `tsx server.ts`

- Runs a TypeScript file directly.
- Transpiles TypeScript on the fly.
- No build step required.
- Ideal for development and quick testing.

```bash
tsx server.ts
```

## `tsc -b && node server.js`

- `tsc -b` compiles the TypeScript project into JavaScript.
- `node` runs the generated JavaScript file.
- Produces build artifacts (`.js` files).
- Preferred for production deployments.

```bash
tsc -b
node server.js
```

## Quick Comparison

| `tsx` | `tsc + node` |
|--------|-------------|
| No build step | Requires compilation |
| Faster development workflow | Better for production |
| Runs `.ts` directly | Runs compiled `.js` |
| No output files | Generates build files |

### Rule of Thumb

- **Development:** `tsx server.ts`
- **Production:** `tsc -b && node <compiled-file>.js`