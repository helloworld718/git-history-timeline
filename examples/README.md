# Examples

This folder contains demo and test files for the Git History Timeline widget.

## Files

- **`widget-demo.html`** - Interactive showcase of the animated widget with different configurations
- **`widget-embed-test.html`** - Automated tests for widget embedding scenarios

## Usage

These files reference the generated widget in `../dist/widget.html`. Make sure to run the build first:

```bash
npm run widget
```

Then open the examples in your browser:

```bash
open examples/widget-demo.html
```

## Note

These are static files used for demonstration and testing. They are committed to the repository, unlike the generated files in `dist/` which are git-ignored.
