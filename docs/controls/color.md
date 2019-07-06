---
layout: doc
title: Color Control
subtitle: A flexible Colorpicker control for the WordPress Customizer
tags: [controls]
---

The `kirki-color` control allows you to add a customized color control in the WordPress Customizer.

The control will automatically detect and use the color-palette defined by your theme as per the [WordPress Docs](https://wordpress.org/gutenberg/handbook/designers-developers/developers/themes/theme-support/), and the defined colors will be used for the control's palette options.  

It is also possible to [create a hue-only control](https://github.com/kirki-framework/control-color/wiki/Adding-a-hue-only-control).

## Installation Using Composer

You can install this control using Composer:

```bash
composer require kirki-framework/control-color
```

In your theme's `functions.php` file make sure you include the autoloader:
```php
require_once get_parent_theme_file_path( 'vendor/autoload.php' );
```

## Installation without Composer

If you are not using composer, you will need to download the repository as well as its dependencies.
We recommend you install these packages in a new folder, for example `inc/customizer/packages` in your theme.  
To download the packages you can download the repositories via `git`:

```bash
git clone https://github.com/kirki-framework/control-color
git clone https://github.com/kirki-framework/control-base
git clone https://github.com/kirki-framework/url-getter
git clone https://github.com/kirki-framework/field
```
Once the repositories are cloned, you can include the files in your theme's `functions.php` file:

```php
// Include url-getter package files.
require_once 'inc/customizer/packages/url-getter/src/URL.php';
// Include control-base package files.
require_once 'inc/customizer/packages/control-base/src/Control/Base.php';
// Include field package files.
require_once 'inc/customizer/packages/field/src/Field.php';
// Include control-color package files.
require_once 'inc/customizer/packages/control-color/src/Control/Color.php';
require_once 'inc/customizer/packages/control-color/src/Field/Color.php';
```

## How to Create a Control Using the Customizer API

To add a control using the customizer API:

```php
/**
 * Add Customizer settings & controls.
 * 
 * @since 1.0
 * @param WP_Customize_Manager $wp_customize The WP_Customize_Manager object.
 * @return void
 */
add_action( 'customize_register', function( $wp_customize ) {

	// Registers the control and whitelists it for JS templating.
	// Only needs to run once, not needed for every control we add.
	$wp_customize->register_control_type( '\Kirki\Control\Color' );

	// Add setting.
	$wp_customize->add_setting( 'my_control', [
		'type'              => 'theme_mod',
		'capability'        => 'edit_theme_options',
		'default'           => '#fff',
		'transport'         => 'refresh', // Or postMessage.
		'sanitize_callback' => [ '\kirki\Field\Color', 'sanitize' ], // Or a custom sanitization callback.
	] );

	// Add control.
	$wp_customize->add_control( new \Kirki\Control\Color( $wp_customize, 'my_control', [
		'label'   => esc_html__( 'My Color Control', 'theme_textdomain' ),
		'section' => 'colors',
		'choices' => [
			'alpha' => true,
		]
	] ) );
} );
```

## How to Create a Control Using the Simplified API

You may use a slightly simplified and abstracted API provided by this control:

```php
new \Kirki\Field\Color( [
	'option_type' => 'theme_mod',
	'capability'  => 'edit_theme_options',
	'default'     => '#fff',
	'transport'   => 'refresh', // Or postMessage.
	'label'       => esc_html__( 'My Color Control', 'theme_textdomain' ),
	'section'     => 'colors',
	'choices'     => [
		'alpha' => true,
	]
] );
```

This is a simple abstraction, allowing you to write less code and achieve the same results as the example from the Customizer API above.

Things to note regarding this abstraction:

* The `Kirki\Field\Color` object will register the control-class, add the setting and the control, all in one step.
* While the default WordPress Customizer API requires hooking in `customize_register`, using the `Kirki\Field\Color` class should not be done inside that hook. There is no need for that, all actions are added automatically by the object itself.
* The sanitization callback is automatically added if left undefined.

## Adding Transparency (alpha) to your control

To add support for `rgba` values, you can add this in your control:
```php
'choices' => [
	'alpha' => true,
]
```

## Creating a Hue-Only Control

To make a control hue-only, you can add the following in the array of arguments you use when defining the control:

```php
'mode' => 'hue'
```

This will remove the color-palette, and users will be presented with a simple slider they can use to select their hue.

Please note that when in hue mode, the control will save an `integer` value. To use this in your theme, you may want to use `hsl(a)` colors in your CSS.
