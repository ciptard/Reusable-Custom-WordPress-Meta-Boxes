Reusable Custom WordPress Meta Boxes
====================================

Contributors: [tammyhart](http://github.com/tammyhart)

Original Tutorial Series: (http://wp.tutsplus.com/author/tammy/)


Description
-----------

This project was originally a tutorial for WPTuts+, but as I continued to use the code in my own 
projects, I kept improving it and making the resusable part even better. Rather than update the 
tutorial, I decided to make it an open source project here where developers can keep up with 
improvements as well as contribute their own.

This project creates a series of functions that make it easy to create a custom meta box for any 
post type using a master switch case for the meta box and fields HTML and an array containing 
the data for the fields you want to use.


### Fields Included

* Text Input
* Textarea
* Single checkbox
* Select box
* Radio group
* Checkbox group
* Taxonomy Select box
* Post ID select box
* jQuery UI Date input
* jQuery UI Slider
* Image ID field
* Repeatable & Sortable Text inputs


Usage
-----

These files are written from the perspecitve of being used in a theme. 

1. Add all of the files to your theme directory
2. Include the main meta_box.php file in your functions.php
4. Use the functions created in meta_box.php to add meta boxes to any post type (see Examples section below)


Examples
--------

### A sample meta box that uses all of the field types for the "post" post type

	function thd_sample_meta_box_fields() {
		$prefix = 'sample_';
	
		$fields = array(
			array( // Text Input
				'label'	=> 'Text Input', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'text', // field id and name
				'type'	=> 'text' // type of field
			),
			array( // Textarea
				'label'	=> 'Textarea', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'textarea', // field id and name
				'type'	=> 'textarea' // type of field
			),
			array( // Single checkbox
				'label'	=> 'Checkbox Input', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'checkbox', // field id and name
				'type'	=> 'checkbox' // type of field
			),
			array( // Select box
				'label'	=> 'Select Box', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'select', // field id and name
				'type'	=> 'select', // type of field
				'options' => array ( // array of options
					'one' => array ( // array key needs to be the same as the option value
						'label' => 'Option One', // text displayed as the option
						'value'	=> 'one' // value stored for the option
					),
					'two' => array (
						'label' => 'Option Two',
						'value'	=> 'two'
					),
					'three' => array (
						'label' => 'Option Three',
						'value'	=> 'three'
					)
				)
			),
			array ( // Radio group
				'label'	=> 'Radio Group', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'radio', // field id and name
				'type'	=> 'radio', // type of field
				'options' => array ( // array of options
					'one' => array ( // array key needs to be the same as the option value
						'label' => 'Option One', // text displayed as the option
						'value'	=> 'one' // value stored for the option
					),
					'two' => array (
						'label' => 'Option Two',
						'value'	=> 'two'
					),
					'three' => array (
						'label' => 'Option Three',
						'value'	=> 'three'
					)
				)
			),
			array ( // Checkbox group
				'label'	=> 'Checkbox Group', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'checkbox_group', // field id and name
				'type'	=> 'checkbox_group', // type of field
				'options' => array ( // array of options
					'one' => array ( // array key needs to be the same as the option value
						'label' => 'Option One', // text displayed as the option
						'value'	=> 'one' // value stored for the option
					),
					'two' => array (
						'label' => 'Option Two',
						'value'	=> 'two'
					),
					'three' => array (
						'label' => 'Option Three',
						'value'	=> 'three'
					)
				)
			),
			array( // Taxonomy Select box
				'label'	=> 'Category', // <label>
				// the description is created in the callback function with a link to Manage the taxonomy terms
				'id'	=> 'category', // field id and name, needs to be the exact name of the taxonomy
				'type'	=> 'tax_select' // type of field
			),
			array( // Post ID select box
				'label'	=> 'Post List', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=>  $prefix.'post_id', // field id and name
				'type'	=> 'post_list', // type of field
				'post_type' => array('post','page') // post types to display, options are prefixed with their post type
			),
			array( // jQuery UI Date input
				'label'	=> 'Date', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'date', // field id and name
				'type'	=> 'date' // type of field
			),
			array( // jQuery UI Slider
				'label'	=> 'Slider', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'slider', // field id and name
				'type'	=> 'slider', // type of field
				'min'	=> '0', // lowest possible number
				'max'	=> '100', // highest possible number
				'step'	=> '5' // how the slider steps as it is dragged
			),
			array( // Image ID field
				'label'	=> 'Image', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'image', // field id and name
				'type'	=> 'image' // type of field
			),
			array( // Repeatable & Sortable Text inputs
				'label'	=> 'Repeatable', // <label>
				'desc'	=> 'A description for the field.', // description
				'id'	=> $prefix.'repeatable', // field id and name
				'type'	=> 'repeatable' // type of field
			)
		);
		
		return $fields;
	}
	
	// Header JS
	add_action('admin_head', 'thd_sample_header_js');
	function thd_sample_header_js() {
		echo thd_meta_box_admin_head(thd_sample_meta_box_fields(), 'post');
	}
	
	// Add meta box
	add_action('admin_menu', 'thd_sample_add_box');
	function thd_sample_add_box() {
		add_meta_box('sample_info', 'Listing Info', 'thd_sample_show_box', 'post', 'normal', 'high');
	}
	// Callback function to show fields in meta box
	function thd_sample_show_box() {
		thd_meta_box_callback(thd_sample_meta_box_fields(), 'post');
	}
	
	
	// Save data from meta box
	add_action('save_post', 'thd_sample_save_data');
	function thd_sample_save_data($post_id) {
		thd_meta_box_save($post_id, thd_sample_meta_box_fields(), 'post');
	}
	
	// Remove the Town Taxonomy box from the write post page
	add_action( 'admin_menu' , 'thd_re_remove_towndiv' );
	function thd_sample_remove_towndiv() {
		remove_meta_box('towndiv', 'post', 'side');
	}


### Add listing information to a Real Estate post type "real-estate"

	function thd_re_meta_box_fields() {
		$prefix = 're_';
	
		$fields = array(
			array(
				'label'	=> 'Town',
				'desc'	=> 'What town is this listing in or nearby?',
				'id'		=> 'town',
				'type'	=> 'tax_select'
			),
			array(
				'label'	=> 'Address',
				'desc'	=> 'Street, town, state, and zip code.',
				'id'		=> $prefix.'address',
				'type'	=> 'textarea'
			),
			array(
				'label'	=> 'Size',
				'desc'	=> 'Square footage.',
				'id'		=> $prefix.'size',
				'type' 	=> 'text'
			),
			array(
				'label' => 'Bedrooms',
				'desc'	=> '',
				'id'		=> $prefix.'beds',
				'type'	=> 'slider',
				'min'		=> '1',
				'max'		=> '8',
				'step'	=> '1'
			),
			array(
				'label'	=> 'Bathrooms',
				'desc'	=> '',
				'id'		=> $prefix.'baths',
				'type'	=> 'slider',
				'min'		=> '0',
				'max'		=> '5',
				'step'	=> '.5'
			),
			array(
				'label'	=> 'Acreage',
				'desc'	=> 'Lot size',
				'id'		=> $prefix.'acres',
				'type'	=> 'text'
			),
			array(
				'label'	=> 'Price',
				'desc'	=> '',
				'id'		=> $prefix.'price',
				'type'	=> 'text',
				'value'	=> '$'
			),
			array(
				'label'	=> 'Contact Name',
				'desc'	=> '',
				'id'		=> $prefix.'name',
				'type'	=> 'text'
			),
			array(
				'label'	=> 'Contact Number',
				'desc'	=> '',
				'id'		=> $prefix.'phone',
				'type'	=> 'text'
			),
			array(
				'label'	=> 'Contact Email',
				'desc'	=> '',
				'id'		=> $prefix.'email',
				'type'	=> 'text'
			)
		);
		
		return $fields;
	}
	
	// Header JS
	add_action('admin_head', 'thd_re_listing_header_js');
	function thd_re_listing_header_js() {
		echo thd_meta_box_admin_head(thd_re_meta_box_fields(), 'real-estate');
	}
	
	// Add meta box
	add_action('admin_menu', 'thd_re_listing_add_box');
	function thd_re_listing_add_box() {
		add_meta_box('re_info', 'Listing Info', 'thd_re_listing_show_box', 'real-estate', 'normal', 'high');
	}
	// Callback function to show fields in meta box
	function thd_re_listing_show_box() {
		thd_meta_box_callback(thd_re_meta_box_fields(), 'real-estate');
	}
	
	
	// Save data from meta box
	add_action('save_post', 'thd_re_listing_save_data');
	function thd_re_listing_save_data($post_id) {
		thd_meta_box_save($post_id, thd_re_meta_box_fields(), 'real-estate');
	}
	
	// Remove the Town Taxonomy box from the write post page
	add_action( 'admin_menu' , 'thd_re_remove_towndiv' );
	function thd_re_remove_towndiv() {
		remove_meta_box('towndiv', 'real-estate', 'side');
	}


Changelog
---------

### 0.1 (March 31, 2012)
* First release