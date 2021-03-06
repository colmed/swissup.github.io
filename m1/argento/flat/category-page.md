---
layout: default
title: Argento Flat Category Page
description: Argento Flat category page
keywords: "Argento flat, argento, category, listing, label, columns count"
category: Argento
---

# Argento Flat Category Page

### Layout

Argento default theme is using `page_two_columns_right` layout for the category page. In order to
change it to another one you have to create [custom.xml][custom_xml] file with following
instruction:

```xml
<catalog_category_default>
    <update handle="page_two_columns_right"/>
</catalog_category_default>
<catalog_category_layered>
    <update handle="page_two_columns_right"/>
</catalog_category_layered>
```

Possible values for handle variable:

```
page_two_columns_left
page_two_columns_right
page_three_columns
page_one_column
```

### Description

![Collapsed category description](/images/argento/flat/category-page/collapsed_description.png)

Category description is shown as collapsed block, when its height is longer than 250px. If you would like to disable description collapsing, you can add the following code to [custom.xml][custom_xml]:

```xml
<remove name="collapsed_category_description"/>
```

## Product Listing

![Product Listing](/images/argento/flat/category-page/product-listing.png)

#### Columns count

Columns count can be configured with [custom.xml][custom_xml].
Add the following code to the custom.xml file:

```xml
<products_four_columns>
    <block type="catalog/product_list_toolbar" name="product_list_toolbar" template="catalog/product/list/toolbar.phtml">
        <block type="page/html_pager" name="product_list_toolbar_pager"/>

        <action method="setDefaultGridPerPage"><limit>4</limit></action>
        <action method="addPagerLimit"><mode>grid</mode><limit>4</limit></action>
        <action method="addPagerLimit"><mode>grid</mode><limit>8</limit></action>
        <action method="addPagerLimit"><mode>grid</mode><limit>12</limit></action>

        <action method="setDefaultListPerPage"><limit>4</limit></action>
        <action method="addPagerLimit"><mode>list</mode><limit>8</limit></action>
        <action method="addPagerLimit"><mode>list</mode><limit>16</limit></action>
        <action method="addPagerLimit"><mode>list</mode><limit>32</limit></action>
    </block>

    <reference name="product_list">
        <action method="setColumnCount"><columns>4</columns></action>
        <action method="setToolbarBlockName"><name>product_list_toolbar</name></action>
    </reference>
    <reference name="search_result_list">
        <action method="setColumnCount"><columns>4</columns></action>
        <action method="setToolbarBlockName"><name>product_list_toolbar</name></action>
    </reference>
</products_four_columns>

<catalog_category_layered>
    <update handle="products_four_columns"/>
</catalog_category_layered>
<catalog_category_default>
    <update handle="products_four_columns"/>
</catalog_category_default>
<catalogsearch_result_index>
    <update handle="products_four_columns"/>
</catalogsearch_result_index>
<catalogsearch_advanced_result>
    <update handle="products_four_columns"/>
</catalogsearch_advanced_result>
```

#### Image size

Every Argento theme since package version 1.9.5 has easy-to-use admin interface to maintain product images resizing at category page.

Open Magento Admin and go to `System` → `Configuration`. Find `TM Argento Themes` section there and choose `Flat`.

![Product Listing image size](/images/argento/default/listing-image-settings.png)

Product listing can have two modes - grid and list. And here you can specify image resizing values for both of this modes separately.

One exception is image background. It applies for both grid and list modes.

`Product image width` and `Product image height` are pretty self explanatory names.

Option `Keep frame size of product image` can be tricky for some users. That is why we wrote an article that helps you to understand effect of this option - ["Keep frame" explanation](/m1/argento/keep-frame/).

#### Image size (deprecated)

> Deprecated since Argento package 1.9.0. Do not use it if your Argento package has version 1.9.x or higher.

Follow the steps below to change the image's size on the category view page:

 1. Copy product list template to the `flat_custom` folder:

    ```
    cp app/design/frontend/argento/flat/template/catalog/product/list.phtml app/design/frontend/argento/flat_custom/template/catalog/product/list.phtml
    ```

 2. Change the image dimensions in copied file.

    Find the following lines:

    ```php
    <img id="product-collection-image-<?php echo $_product->getId(); ?>"
        src="<?php echo $this->helper('catalog/image')->init($_product, 'small_image')->keepFrame(false)->resize(270); ?>"
        srcset="<?php echo $this->helper('catalog/image')->init($_product, 'small_image')->keepFrame(false)->resize(270); ?> 1x, <?php echo $this->helper('catalog/image')->init($_product, 'small_image')->keepFrame(false)->resize(270 * 2); ?> 2x"
        alt="<?php echo $this->stripTags($this->getImageLabel($_product, 'small_image'), null, true) ?>"
    />
    ```

    Replace them with:

    ```php
    <img id="product-collection-image-<?php echo $_product->getId(); ?>"
        src="<?php echo $this->helper('catalog/image')->init($_product, 'small_image')->keepFrame(true)->resize(270, 270); ?>"
        srcset="<?php echo $this->helper('catalog/image')->init($_product, 'small_image')->keepFrame(true)->resize(270, 270); ?> 1x, <?php echo $this->helper('catalog/image')->init($_product, 'small_image')->keepFrame(true)->resize(270 * 2, 270 * 2); ?> 2x"
        alt="<?php echo $this->stripTags($this->getImageLabel($_product, 'small_image'), null, true) ?>"
    />
    ```

    Remember to make the same operation for the image in grid and list modes.

#### Product labels

The display of the label on product is powered by [Prolabels module](/m1/extensions/prolabels/). You can add custom label or assign it to any items on the Category page at `Templates-Master > Prolabels` backend page.

[custom_xml]: /m1/argento/theme-customization/small-changes/#custom-layout-update-file "custom.xml layout"
