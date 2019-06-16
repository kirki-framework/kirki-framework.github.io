---
layout: doc
title: Color Control
subtitle: A flexible Colorpicker control for the WordPress Customizer
tags: [controls]
---

The `kirki-color` control allows you to add a customized color control in the WordPress Customizer and uses [iro.js] for the colorpicker.

The control will automatically detect and use the color-palette defined by your theme as per the [WordPress Docs](https://wordpress.org/gutenberg/handbook/designers-developers/developers/themes/theme-support/).  
Visually the palette resembles the palette selector that users are accustomed to from the WordPress Editor.

<img src="https://user-images.githubusercontent.com/588688/57177708-aaf32f00-6e6f-11e9-91c9-046865f95939.png" alt="Color control collapsed" width="300">

Users can select a color by clicking on a color from the palette, or select a custom color by clicking on the "Select Color" link.  
They can enter a custom value in the textfield, or use the colorpicker to select their color. The colorpicker is also able to [handle `rgba` colors](https://github.com/kirki-framework/control-color/wiki/Adding-transparency-support).

<img src="https://user-images.githubusercontent.com/588688/57177713-c100ef80-6e6f-11e9-8c51-6cac6da07fed.png" alt="Color control with alpha channel - expanded" width="300">

It is also possible to [create a hue-only control](https://github.com/kirki-framework/control-color/wiki/Adding-a-hue-only-control).

<img src="https://user-images.githubusercontent.com/588688/57177723-d413bf80-6e6f-11e9-90dc-a9ef415b3fdb.png" alt="Hue-only colorpicker" width="300">

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

If you are not using composer, you can download the control from [this repository](https://github.com/kirki-framework/control-color).  
Once you download - or clone - the repository, place the files in your theme. You can then include the files by adding the following lines in your `functions.php` file:

```php
require_once 'control-color/src/Control/Color.php';
require_once 'control-color/src/Field/Color.php';
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

This is a simple abstraction, allowing you to write less code and achieve the same results as the example in the [customizer-api](https://github.com/kirki-framework/control-color/wiki/Create-Control-Using-the-Customizer-API) example.

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

<img src="https://user-images.githubusercontent.com/588688/57177723-d413bf80-6e6f-11e9-90dc-a9ef415b3fdb.png" alt="Hue-only colorpicker" width="300">

This will remove the color-palette, and users will be presented with a simple slider they can use to select their hue.

Please note that when in hue mode, the control will save an `integer` value. To use this in your theme, you may want to use `hsl(a)` colors in your CSS.

## Updating `iro.js` in the control

To update scripts you can run the following command from a terminal:

```
rm -rf node_modules && npm install --only=production
```