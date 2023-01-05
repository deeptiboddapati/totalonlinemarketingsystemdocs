---
sidebar_label: 'How to build the boilerplate'
sidebar_position: 1
---

# How to build the boilerplate


1. Generate a block with npx @wordpress/create-block

If you want to just create a block without a plugin there is an option for it.

npx @wordpress/create-block --no-plugin

If you want to create a brand new block category run it in /plugins.
https://developer.wordpress.org/block-editor/reference-guides/packages/packages-create-block/

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

Next we must enable Style Lint and get it to format as we type as well as on save.
To enable StyleLint you must first open it's settings file. 

Find the settings file by going to File->Preferences->Settings-> Search Stylelint -> Press 'edit in settings.json.

First add the following data to enable Stylelint
"stylelint.enable": true ,

Next, to ensure that Stylelint will use the configuration file specified in the directory change Stylelint setting for stylelint.config. 
"stylelint.config": null,

Husky
		"prepare": "cd ../../../ && husky install plugins/trunk/site-heros"