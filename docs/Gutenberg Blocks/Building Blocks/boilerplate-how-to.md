---
sidebar_label: 'How to build the boilerplate'
sidebar_position: 1
---

# How to build the boilerplate


1. Generate a block with WordPress Scripts
2. Add the ES lint extension to your editor.
3. Npm install WP eslint-plugin
4. set up .eslintrc file in top level
5. Go to file -> preferences -> Settings -> search format on save and check the box
6. npm install @wordpress/prettier-config --save-dev
7. "prettier": "@wordpress/prettier-config"
8. eslint-with formatting
9. https://github.com/prettier/eslint-config-prettier
10. https://developer.wordpress.org/block-editor/reference-guides/packages/packages-eslint-plugin/
11. Style lint extension go to file -> preferences -> extensions or Control + Shift + X 
12. https://developer.wordpress.org/block-editor/reference-guides/packages/packages-stylelint-config/
    npm install @wordpress/stylelint-config --save-dev
	"stylelint": {
		"extends": "@wordpress/stylelint-config/scss"
	},