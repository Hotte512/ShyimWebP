# WebP Support for Shopware

[WebP Browser-Support](http://caniuse.com/#search=webp)

This plugin generates additional webp Thumbnails and uses is in various locations like detail, listing, emotion as preferred type using html5 picture tag. Browsers which does not support webp, will get normally jpg. 
**After installation of the Plugin regenerating all thumbnails is required.**

# Requirements

* Shopware Version 5.3+
* any webp encoder
  * php-gd with webp support
  * cwebp executable in PATH

# Installation

* Checkout Plugin in `/custom/plugins/ShyimWebP`
* Download google binaries if neccessary `php bin/console shyim:webp:download-google-binaries`
* Generate all Thumbnails new with ``php bin/console sw:thumbnail:generate -f``


# Compatibility to LazyLoading Plugin available on Community-Store
With small changes this Plugin is compatible with LazyLoader https://store.shopware.com/ies6685271780164/lazy-loading.html

* Make change to custom/plugins/IesLazyLoading/Resources/views/frontend/listingproduct-box/box-big-image.tpl 
* Make change to custom/plugins/IesLazyLoading/Resources/views/frontend/listingproduct-box/product-image.tpl
* Take Care about UPDATES of LazyLoader! I will inform the Plugin developer, may be it will be added to the Plugin. - If so You'll read it here!

box-big-image.tpl:
```
{extends file="parent:frontend/listing/product-box/box-big-image.tpl"}

{block name="frontend_listing_box_article_image_picture_element"}	
	<picture>
        {if isset($sArticle.image.thumbnails[1].webp)}
            <source data-srcset="{$sArticle.image.thumbnails[1].webp.sourceSet}" type="image/webp">
        {/if}
        <img class="lazy"
        src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw=="
        {if $iesLazyLoadingEffect}data-effect="{$iesLazyLoadingEffect}"{/if}
        {if $iesLazyLoadingEffectTime}data-effectTime="{$iesLazyLoadingEffectTime}"{/if}
        data-src="{$sArticle.image.thumbnails[1].source}"
        data-srcset="{$sArticle.image.thumbnails[1].sourceSet}"
        alt="{$desc}"
        title="{$desc|truncate:160}"
    	/>
    </picture>
    {block name='frontend_listing_box_article_image_picture_element_no_script'}
        <noscript>{$smarty.block.parent}</noscript>
    {/block}
{/block}
```

product-image.tpl:
```
{extends file="parent:frontend/listing/product-box/product-image.tpl"}

{block name='frontend_listing_box_article_image_picture_element'}
    {$imageSize = $iesLazyLoadingProductImageSize}
    {if !$imageSize}
        {$imageSize = 0}
    {/if}
	<picture>
        {if isset($sArticle.image.thumbnails[0].webp)}
            <source data-srcset="{$sArticle.image.thumbnails[0].webp.sourceSet}" type="image/webp">
        {/if}
        <img class="lazy"
        src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw=="
        {if $iesLazyLoadingEffect}data-effect="{$iesLazyLoadingEffect}"{/if}
        {if $iesLazyLoadingEffectTime}data-effectTime="{$iesLazyLoadingEffectTime}"{/if}
        data-src="{$sArticle.image.thumbnails[$imageSize].source}"
        data-srcset="{$sArticle.image.thumbnails[$imageSize].sourceSet}"
        alt="{$desc}"
        title="{$desc|truncate:160}"
    	/>
    </picture>
    {block name='frontend_listing_box_article_image_picture_element_no_script'}
        <noscript>{$smarty.block.parent}</noscript>
    {/block}
{/block}
```
