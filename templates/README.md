# Templates

This folder contains HTML templates used during the build process.

## Files

- **`widget.html.template`** - Template for the animated widget with `{{WIDGET_DATA}}` placeholder

## Build Process

The `widget.html.template` is processed by `src/build-widget.js`:

1. Reads commit data from `dist/widget-data.json`
2. Injects data into the `{{WIDGET_DATA}}` placeholder
3. Outputs the final widget to `dist/widget.html`

## Editing

When making changes to the widget:

1. Edit `templates/widget.html.template`
2. Ensure the `{{WIDGET_DATA}}` placeholder remains intact
3. Run `npm run widget` to regenerate `dist/widget.html`

The template is committed to the repository, while the generated output in `dist/` is git-ignored.
