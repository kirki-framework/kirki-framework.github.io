---
layout: doc
title: Customizer API
subtitle: Using the Customizer API to add Controls
tags: [api]
---

If you prefer using the default WordPress Customizer APIs to add your controls instead of using the simplified Kirki API, Kirki allows you to use its controls directly. To learn more about the Customizer API, you can read the official [Theme Handbook](https://developer.wordpress.org/themes/customize-api/).

Example of adding a color control using the WordPress Customizer API:

```php
/**
 * Add Customizer settings & controls.
 * 
 * @since 1.0
 * @param WP_Customize_Manager $wp_customize The WP_Customize_Manager object.
 * @return void
 */
add_action( 'customize_register', function( $wp_customize ) {

    /**
     * Registers the control and whitelists it for JS templating.
     * Only needs to run once per control-type/class,
     * not needed for every control we add.
     */
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
