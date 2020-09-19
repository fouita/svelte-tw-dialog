# Svelte and tailwindcss Dialog component

Dialog component built with svelte and tailwindcss, show modal or info messages!

# Installation

```bash
$npm i -D @fouita/dialog
```

# Usage

To show the dialog component we just need to set true to the `visible` prop

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-simple.jpg)

```html
<script>
 import Dialog from '@fouita/dialog'
</script>

<Dialog visible title="Simple Title" body="Simple message!" />
```


## Show and hide + Filter

If we want to show the Dialog when clicking on a button, we better bind the value with `bind:visible={value}` prop.

We can use also `filter={false}` to show the dialog without background 

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-filter.jpg)

```html
<script>
 import Dialog from '@fouita/dialog'
 let visible = false
 function toggle(){
     visible = !visible
 }
</script>

<button class="px-4 py-2 m-2 border" on:click={toggle}>Toggle</button>
<Dialog bind:visible title="Simple Title" body="Simple message!" />
```


## Change round and color

To change the rounding style we can use tailwindcss rounding suffixes `none, sm, md, lg` and to change the color we can from tailwindcss color names

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-rounded.jpg)

```html
<Dialog rounded=sm color=pink visible title="Simple Title" body="Simple message!" />
```

You can also invert colors by adding `inverted` prop

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-inverted.jpg)

```html
<Dialog rounded=sm inverted color=pink visible title="Simple Title" body="Simple message!" />
```


## Add custom elements

To add custom elements in the title, we can use `icon` prop, and to update the body with html we can just write inside the Dialog tag

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-custom.jpg)

```html
<script>
 import Dialog from '@fouita/dialog'
 import {LockIcon} from 'svelte-feather-icons'
</script>

<Dialog visible title="With Icon" icon={LockIcon}>
    <div class="p-4 text-center w-64 h-64 text-2xl">
        Cusom Body
    </div>
</Dialog>
```

## Dialog position & align

`position` prop indicates the starting point when showing a dialog, there is 3 positions `top, middle, bottom`

`align` prop handles the horizontal position of the component `left, center, right`

by default we use `middle` and `center` to have the dialog in the center of the page

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-bottom-left.jpg)

```html
<Dialog visible title="Simple Title" inverted position=bottom align=left rounded=none >
    <div class="p-4 text-center w-64 h-64 text-2xl">
        Cusom Body
    </div>
</Dialog>
```

The follwing is an example of dialog that popsup from the bottom center of the page

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-success-msg.jpg)


```html
<script>
	import Chip from '@fouita/chip'
    import Dialog from '@fouita/dialog'			
    import {CheckCircleIcon} from 'svelte-feather-icons'

    let visible = true
</script>

<Dialog bind:visible position=bottom align=center filter={false} rounded="md" >
    <Chip size="xl" rounded="none" color="teal" class="mx-0 my-0" onDelete={() => dialog4=false} >
        <div slot="avatar">
            <CheckCircleIcon class='w-5 h-5 mx-2' />
        </div>
        <div class="py-2">
            This is a success messsage
            <div class="text-sm font-base">
                More information about the message can be found <u>here</u>
            </div>
        </div>
    </Chip>
</Dialog>
```

Here is another example to show a list of icons in a dialog box

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-icons.jpg)

```html
<script>
    import Dialog from '@fouita/dialog'
    import {LinkedinIcon,FacebookIcon,TwitterIcon,SlackIcon} from 'svelte-feather-icons'
</script>
<Dialog visible filter={false} align=right color=purple >
    <div class="p-4 text-center w-12 h-48 text-purple-600 text-2xl flex flex-col items-center">
        <LinkedinIcon class="w-6 h-6 m-2 cursor-pointer hover:text-purple-800" />
        <FacebookIcon class="w-6 h-6 m-2 cursor-pointer hover:text-purple-800" />
        <TwitterIcon class="w-6 h-6 m-2 cursor-pointer hover:text-purple-800" />
        <SlackIcon class="w-6 h-6 m-2 cursor-pointer hover:text-purple-800" />
    </div>
</Dialog>
```


## Custom width and height

With position we can also indicate the width and the height of the dialog by using `w` and `h` props

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-w-full.jpg)

```html
<Dialog position=bottom w=full filter={false} rounded=none >
 ...
</Dialog> 
```

or we can show a side bar like the following, we used `closable={false}` to hide the `X` icon and disable default closing 

![fouita dialog](https://cdn.fouita.com/assets/pics/template/dialog/dialog-side-menu.jpg)

```html
<Dialog visible h=full title="Side Menu" closable={false} inverted align=left rounded=none >
    <div class="p-4 text-center w-64 h-64 text-2xl">
        Cusom Menu
    </div>
</Dialog>
```


# About

[Fouita : UI framework for svelte + tailwind components](https://fouita.com)

