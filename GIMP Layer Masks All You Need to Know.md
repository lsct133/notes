---
title: "GIMP Layer Masks: Everything You Need to Know"
source: https://daviesmediadesign.com/gimp-layer-masks-everything-you-need-to-know/
description: This in-depth GIMP Help Article covers everything you need to know about layer masks. From what a layer mask is, to how it works, and more.
tags:
  - clippings
---
In my last GIMP Help article, I covered the topic of [layer transparency in GIMP](https://daviesmediadesign.com/gimp-layers-layer-transparency/) – explaining that parts of a layer can be erased to reveal either a color (when no alpha channel is applied to a layer) or a transparent background. The article covered the concept by erasing pixels directly on a layer, which I mentioned was a more destructive form of editing.

However, what do you do when you want to erase or hide part of a layer in a non-destructive way?

The simple answer to this is that you use a *layer mask.*

## What is a Layer Mask?

By the GIMP team’s definition, layer masks “allow you to selectively modify the opacity (transparency) of the layer \[layer masks\] belong to. This differs from the use of the layer Opacity slider as a mask has the ability to selectively modify the opacity of different areas across a single layer.”

To break this down into even simpler terms, you need to understand some simple concepts about layer masks.

For starters, layer masks are placed on top of your original layer as a sort of layer add-on. Once placed on top of your layer, the layer mask can be one of three colors – black, white, or gray. These colors, when on a layer mask, represent full transparency, full opacity, or partial transparency, respectively. So, when I paint black on my layer mask, any pixels below that area where I painted black will now be hidden on the layer that contains the layer mask. If I paint gray on the layer mask, the pixels from that layer will be partially transparent. If I paint white on the layer mask, those pixels will be fully opaque – so it will appear as if nothing has happened to that part of the layer.

So, a layer mask has one of 3 jobs – be totally transparent, be totally opaque, or be partially transparent.

To further clarify this concept, let me show you an example.

## Adding a Layer Mask to An Image

![GIMP Layers Masks Layers](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Layers-Masks-Layers.jpg)

GIMP Layers Masks Layers

In GIMP, I have a composition open with two image layers – the top layer is the “Model in Red Chair” image I have been working with throughout the Layers series, and below that layer is an image layer titled “Zion” (outlined in green in the image above). The bottom layer features a rock landscape found in Zion National Park.

I want to replace the backdrop of the Model in Red Chair photo with the Zion photo. In other words, I want to hide the top portion of the Model in Red Chair photo (without hiding the model), and have the bottom photo layer showing instead.

![How to Add a Layer Mask in GIMP Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/How-to-Add-a-Layer-Mask-in-GIMP-Tutorial.jpg)

How to Add a Layer Mask in GIMP Tutorial

To do this, I can use a layer mask on the top photo layer. To add a layer mask, I simply right click on the layer in the layers panel that I want to add the mask to and click “Add Layer Mask” (denoted by the red arrow in the photo above).

![Initialize Layer Mask to White GIMP Layer Masks](https://daviesmediadesign.com/wp-content/uploads/2019/08/Initialize-Layer-Mask-to-White-GIMP-Layer-Masks.jpg)

Initialize Layer Mask to White GIMP Layer Masks

The “Add Layer Mask” dialogue box will appear, which allows you to choose from a variety of layer mask options. I will get into these options a bit more later on in this article. For now, under “Initiatlize Layer Mask to,” I’ll choose the first option “White (full opacity)” (red arrow in the photo above). Click the “Add” button to add the layer mask (green arrow).

## Black, Gray, and White on a Layer Mask

![White Layer Mask with Green Layer Outline GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/White-Layer-Mask-with-Green-Layer-Outline-GIMP.jpg)

White Layer Mask with Green Layer Outline GIMP

Now, over in our Layers panel, you should see two thumbnails for your Model in Red Chair layer. The first thumbnail, of course, is the original image thumbnail. The second thumbnail is a solid white thumbnail (denoted by the red arrow in the photo above). This second thumbnail represents your layer mask. You can click on this white thumbnail in order to make the layer mask active. When your layer mask is active, the border around your layer will change from a yellow dotted line to a green dotted line (blue arrow).

*Tip:* if you want to change the size of your image thumbnail preview in the Layers panel, click on the triangle menu in the top right corner of the Layers panel. Go to “Preview Size” and choose whatever size works best for you (mine is set to medium, which is the default thumbnail size).

Because, when we created our layer mask, we selected “White (full opacity)” as our layer mask option, our layer mask shows up as white in the thumbnail preview. This thumbnail preview will always contain one or any of the three colors mentioned above (black, white, or gray) – it will never contain any other colors (like green, blue, etc.). You CAN paint another color on the layer mask, but only the black, white, and/or gray elements from that color will be kept.

One thing you may be asking yourself right now, though, is why hasn’t anything changed after adding the layer mask? The reason is that white represents full opacity. This means that all pixels in the layer below the layer mask are still visible, or fully opaque.

![Filling a GIMP Layer Mask with Black](https://daviesmediadesign.com/wp-content/uploads/2019/08/Filling-a-GIMP-Layer-Mask-with-Black.jpg)

Filling a GIMP Layer Mask with Black

If I grab my Bucket Fill tool from my tool box (red arrow), change my foreground color to black (green arrow), and fill in my layer mask with this black color by clicking anywhere in my image window, my entire Model in Red Chair layer will become transparent. It will disappear. That’s because the color black hides all of the pixels that the layer mask is on. As a result, the Zion photo in the layer below has now been revealed. You will also see that my layer mask thumbnail is now black instead of white (blue arrow).

I’ll hit ctrl+z on my keyboard (Edit>Undo) to undo my changes by one step. This will bring my white layer mask back.

![Change Foreground Color to Middle Gray](https://daviesmediadesign.com/wp-content/uploads/2019/08/Change-Foreground-Color-to-Middle-Gray.jpg)

Change Foreground Color to Middle Gray

Now, with my Bucket Fill tool still selected, I will change my foreground color (red arrow in the photo above) to a middle gray color (you can copy this color by typing 777777 in your HTML notation – green arrow). I’ll click OK to set this gray color as my foreground color.

![Gray Layer Mask GIMP Layers Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/Gray-Layer-Mask-GIMP-Layers-Tutorial.jpg)

Gray Layer Mask GIMP Layers Tutorial

If I fill the layer mask in with this gray color, my Model in Red Chair image will become partially transparent. In other words, it is partially see-through – revealing the photo below without totally disappearing. My layer mask thumbnail is also now gray (red arrow).

I’ll hit ctrl+z once again to go back to having a white layer mask.

## Using Layer Masks to Create Effects

We now know what a layer mask is, as well as how it works. This still begs the question: why is it useful?

All three examples from the previous section showed how a layer mask reacts to being filled in entirely with either a white, black, or gray solid color. However, layer masks really become useful when they begin to use a combination of these three colors, as well as take on specific shapes and shades to blend objects from one images into another image.

If you’ll recall, we were looking to make it appear as if the Model in Red Chair was actually positioned in Zion. We want to erase the top portion of the image, revealing the Zion image from the lower layer.

I can accomplish this by only painting areas of black on the parts of the image that I want hidden. I can use any of the paint tools in GIMP to accomplish this – but the most useful one in this case is likely to be the Paintbrush tool.

![Paintbrush Tool with Hardness 100 Brush](https://daviesmediadesign.com/wp-content/uploads/2019/08/Paintbrush-Tool-with-Hardness-100-Brush.jpg)

Paintbrush Tool with Hardness 100 Brush

So, I’ll grab my paintbrush tool (red arrow in the above photo), and will choose the brush head from my brush panel in the Tool Options with Hardness 100 (green arrow). I’ll also reset my foreground color to black.

![Painting Black on a Layer Mask in GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Painting-Black-on-a-Layer-Mask-in-GIMP.jpg)

Painting Black on a Layer Mask in GIMP

If I begin painting painting on the layer mask with black, all of the areas where I paint will now become transparent (as seen in the area pointed out by the green arrow). This will reveal the Zion image below. If you look at my thumbnail (red arrow), you can see all of the areas where I have painted black on the layer mask. Everywhere else is still white.

The issue here is that the edges where the black paint and white background meet on the layer mask are really sharp – I’d prefer them to be blended a bit better so that the photos can gently transition into one another (this typically looks more realistic, depending on what you are working on).

![Selecting a Hardness 025 Brush Head GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Selecting-a-Hardness-025-Brush-Head-GIMP.jpg)

Selecting a Hardness 025 Brush Head GIMP

To make the edges softer, I can simply grab a softer brush (I went with the Hardness 25 brush – denoted by the red arrow) and continue painting.

![Painting with a Soft Brush on a GIMP Layer Mask](https://daviesmediadesign.com/wp-content/uploads/2019/08/Painting-with-a-Soft-Brush-on-a-GIMP-Layer-Mask.jpg)

Painting with a Soft Brush on a GIMP Layer Mask

Now, instead of having really sharp edges wherever I paint, I have much softer edges and a better transition from one photo to the next. This soft transition occurs because the softer brush is painting black in the middle of the brush head, and gray towards the outer edges of the brush head. The gray eventually fades to white. In other words, we have a transition from full transparency to partial transparency to full opacity happening on our brush.

![Blending Two Photos with GIMP Layer Mask](https://daviesmediadesign.com/wp-content/uploads/2019/08/Blending-Two-Photos-with-GIMP-Layer-Mask.jpg)

Blending Two Photos with GIMP Layer Mask

In the example above, I used the softer brush (Hardness 25) to finish painting all of the upper areas of my layer mask. As a result, the two photos appear blended together (albeit not convincingly – the more time you spend on these sorts of things, the better they will look). You can see the shape of the black area I painted by looking at the layer mask thumbnail in the Layers panel.

## Using Gradients with Layer Masks

As I mentioned, you can use *any* of the paint tools to paint on the layer mask. This includes the Gradient tool. I’ll show you a few examples of this in action.

![Drawing a Gradient on a Layer Mask GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Drawing-a-Gradient-on-a-Layer-Mask-GIMP.jpg)

Drawing a Gradient on a Layer Mask GIMP

I’ll grab my gradient tool from the toolbox (red arrow) and will set the Gradient itself to “Foreground to Transparent” (you can do this by clicking the “Gradient” option in the Tool Options – denoted by the green arrow). I also set the shape of the gradient to Linear (blue arrow). Making sure my foreground color is set to black, I’ll click and drag my mouse on the image window (on the Layer Mask) near the bottom of the photo (yellow arrow).

As you can see, wherever the gradient is black on the layer mask will produce transparency, and wherever the gradient is transparent it will not produce any effect since our layer mask itself is white. This causes the image to slowly fade out at the bottom of the image.

![GIMP Layer Mask with Gradient Thumbnail](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Layer-Mask-with-Gradient-Thumbnail.jpg)

GIMP Layer Mask with Gradient Thumbnail

If I press the enter key to apply my gradient, you’ll see my thumbnail preview for the layer mask (red arrow) shows the white portion of the layer mask transitioning to black towards the bottom via the gradient. You will also see that the Zion photo starts to appear at the bottom of our photo composition. Because the gradient we drew faded to transparency, it did not effect any of the black portions of the layer mask that we painted earlier. As a result, our gradient has been combined with the black areas we painted with our paintbrush to create a compounded effect.

I’ll hit ctrl+z to undo the gradient on the layer mask.

![FG to BG Gradient on GIMP Layer Mask Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/FG-to-BG-Gradient-on-GIMP-Layer-Mask-Tutorial.jpg)

FG to BG Gradient on GIMP Layer Mask Tutorial

For the next example, I’ll change the gradient settings to “Foreground to Background (RGB)” (clicking the Gradient option again in my Gradient Tool Options – denoted by the red arrow), and will also keep the gradient shape set to “Linear.” My foreground color is black, and my background color is white.

Now, when I draw my gradient on the bottom of the composition (still on the layer mask – green arrow), all of the white areas from my gradient cover up the black areas we painted earlier. This causes those transparent areas to become opaque again, and those parts of our image will now display. So, the top portion of our Model in Red Chair image are visible again.

*Note:* if your gradient has black at the top and white at the bottom (i.e. the top portion of your photo disappeared and only the bottom portion is showing), you can click the “Reverse” button in the Gradient tool options to switch your colors. This will make your gradient look like mine.

![White to Black Gradient Layer Mask GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/White-to-Black-Gradient-Layer-Mask-GIMP.jpg)

White to Black Gradient Layer Mask GIMP

If I hit enter to apply the gradient once again, you’ll see our layer mask thumbnail now has white on the top portion and black on the bottom portion (red arrow). All of the black painted areas have been erased.

I’ll hit ctrl+z once again to undo the gradient.

![Change Gradient Type to Radial in GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Change-Gradient-Type-to-Radial-in-GIMP.jpg)

Change Gradient Type to Radial in GIMP

For the last example, I will swap my foreground and background colors so that white is my foreground color, keep the Gradient type set to “Foreground to Background,” and will change the gradient shape to “Radial” (red arrow).

![Drawing a Radial Gradient GIMP Layer Mask Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/Drawing-a-Radial-Gradient-GIMP-Layer-Mask-Tutorial-1.jpg)

Drawing a Radial Gradient GIMP Layer Mask Tutorial

Now, if I click and drag my mouse from the center of my image (red arrow) to one of the outer edges, you’ll see my photo will now fade from the center out to the corners of the image in a circular shape.

I can adjust the midpoint of this gradient to change when the fading occurs (the midpoint appears when you hover your mouse over the line segment connecting your endpoints – not pictured above), as well as adjust either of the endpoints (denoted by the green arrow).

![Adjusting Gradient Stop Color GIMP Layer Mask Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/Adjusting-Gradient-Stop-Color-GIMP-Layer-Mask-Tutorial.jpg)

Adjusting Gradient Stop Color GIMP Layer Mask Tutorial

Additionally, I can click on the line segment in between the end points to add a “stop” (green arrow). By adding a stop, I am essentially adding another color to the gradient. If I change the color of the stop to white (red arrow – both my Left Color and Right Color are white), you’ll see that now more of my Model in Red Chair photo will be visible in the middle of the photo.

![GIMP Layer Mask with Radial Gradient](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Layer-Mask-with-Radial-Gradient.jpg)

GIMP Layer Mask with Radial Gradient

I’ll hit the enter key to apply the gradient. If you look at my layer mask thumbnail (red arrow), you’ll see that my layer mask is now a circular gradient that starts white in the center and transitions to black around the edges.

I’ll hit ctrl+z to undo the circular gradient layer mask and go back to the painted layer mask.

## Context Menu Layer Mask Options

![GIMP Layer Context Menu Layer Masks](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Layer-Context-Menu-Layer-Masks.jpg)

GIMP Layer Context Menu Layer Masks

GIMP comes with some additional options for the layer mask inside of the Layer “Context Menu” (outlined in green in the photo above). This is the official name of the menu that appears when you right click on a layer in your layers panel.

You are already aware of the “Add layer mask” option inside of the Context Menu as this is how we added a layer mask to our image at the beginning of the tutorial.

#### Apply Layer Mask

![Apply Layer Mask GIMP Layers Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/Apply-Layer-Mask-GIMP-Layers-Tutorial.jpg)

Apply Layer Mask GIMP Layers Tutorial

The option below “Add layer mask” is the “Apply layer mask” option (red arrow).

![Layer Mask Applied GIMP Layer Context Menu](https://daviesmediadesign.com/wp-content/uploads/2019/08/Layer-Mask-Applied-GIMP-Layer-Context-Menu.jpg)

Layer Mask Applied GIMP Layer Context Menu

By clicking this, you are essentially merging your layer mask with the layer that the layer mask is on. You are also keeping the effects of the layer mask on the layer. This makes the layer mask destructive as the layer mask is now happening directly on the layer as opposed to on the layer mask. As you can see in the photo above (red arrow), there is no more layer mask thumbnail. However, the benefit of this is that you can now add a new layer mask to the layer and add new layer mask effects.

I’ll hit ctrl+z to undo applying the layer mask.

#### Delete Layer Mask

![GIMP Layer Context Menu Delete Layer Mask](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Layer-Context-Menu-Delete-Layer-Mask.jpg)

GIMP Layer Context Menu Delete Layer Mask

The next option in the Context Menu is to “Delete the layer mask.” This will simply get rid of the layer mask entirely, reverting your image back to its original state before adding the layer mask.

#### Show Layer Mask

![Show Layer Mask GIMP Layer Context Menu](https://daviesmediadesign.com/wp-content/uploads/2019/08/Show-Layer-Mask-GIMP-Layer-Context-Menu.jpg)

Show Layer Mask GIMP Layer Context Menu

Below the option to delete the layer mask in the Context Menu is the option to “Show Layer Mask” (red arrow).

![Show Layer Mask Enabled in GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Show-Layer-Mask-Enabled-in-GIMP.jpg)

Show Layer Mask Enabled in GIMP

This option will display your layer mask on your image window, allowing you to more clearly see what you’ve painted on the mask. This has some serious benefits as you can now clean up any areas of the layer mask (i.e. painting black, white, or grey), giving you a more precise layer mask. When this option is enabled, your layer mask thumbnail will have a green outline around it in the Layers panel (red arrow).

In this case, you can see that I missed several spots when painting with the black.

![Show Layer Mask Painting with Paintbrush GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Show-Layer-Mask-Painting-with-Paintbrush-GIMP.jpg)

Show Layer Mask Painting with Paintbrush GIMP

What I can do is grab my paintbrush tool (blue arrow), switch my foreground color back to black (green arrow), and paint directly on the layer mask displayed in the image window (red arrow). I can see all of the white strips/spots I missed earlier being cleaned up as I paint on them with black.

![Disable Show Layer Mask in GIMP Layers Context Menu](https://daviesmediadesign.com/wp-content/uploads/2019/08/Disable-Show-Layer-Mask-in-GIMP-Layers-Context-Menu.jpg)

Disable Show Layer Mask in GIMP Layers Context Menu

If I right click on my layer to bring up the Context Menu once again, I can click the “Show Layer Mask” option to uncheck it (red arrow). My layer and layer mask now display again (not shown in the photo), and the layer mask has been cleaned up for a better overall effect.

#### Edit Layer Mask

![Edit Layer Mask GIMP Layer Context Menu](https://daviesmediadesign.com/wp-content/uploads/2019/08/Edit-Layer-Mask-GIMP-Layer-Context-Menu.jpg)

Edit Layer Mask GIMP Layer Context Menu

Below the Show Layer Mask option is the “Edit Layer Mask” option (red arrow). When this is checked (or has an “x”) it means you currently have the layer mask selected and are editing the layer mask. When unchecked, it means you have the layer selected and are *not* editing the layer mask.

#### Disable Layer Mask

![Disable Layer Mask GIMP Layer Context Menu](https://daviesmediadesign.com/wp-content/uploads/2019/08/Disable-Layer-Mask-GIMP-Layer-Context-Menu.jpg)

Disable Layer Mask GIMP Layer Context Menu

Next in the Context Menu is the “Disable Layer Mask” option (red arrow).

![Disable Layer Mask Enabled GIMP Layer Context Menu](https://daviesmediadesign.com/wp-content/uploads/2019/08/Disable-Layer-Mask-Enabled-GIMP-Layer-Context-Menu.jpg)

Disable Layer Mask Enabled GIMP Layer Context Menu

When enabled (you’ll know it’s enabled when there is an “x” in the layer context menu – denoted by the red arrow), this option will temporarily turn your layer mask off and make the original layer fully visible (as denoted by the blue arrow). You will see that the layer mask thumbnail will have a red outline around it (green arrow), indicating that the mask is currently disabled.

I will right click on the Model in Red Chair layer to bring up the Context Menu and will click the “Disable Layer Mask” option one more time to uncheck it.

My original layer mask will become enabled again, returning the layer mask effects to my image layer.
#### Mask to Selection

![Mask to Selection GIMP Layer Context Menu](https://daviesmediadesign.com/wp-content/uploads/2019/08/Mask-to-Selection-GIMP-Layer-Context-Menu.jpg)

Mask to Selection GIMP Layer Context Menu

The last layer mask option in the Context Menu, below the Disable Layer Mask option, is the “Mask to Selection” option (red arrow).

![GIMP Mask to Selection on Layer Mask](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Mask-to-Selection-on-Layer-Mask.jpg)

GIMP Mask to Selection on Layer Mask

When I click this option, a selection area will be drawn around the fully opaque, or white, portions of my layer mask (green arrow). This feature is very useful for a variety of reasons – it allows you to to save the selection area as a path, copy and paste the selection area onto another layer (and even convert that selection area to a layer mask – more on that later), or invert the mask and apply effects to everything opposite of your layer mask selection area.

I can deselect this selection area by going to Select>None or by hitting ctrl+shift+a on my keyboard.

## Layer Mask Types

At the start of this tutorial, we created a new layer mask by choosing the “White (full opacity)” option in the Layers dialogue. However, if you’ll remember, there were actually several options available as to the type of layer mask we could create in this layers dialogue.

![GIMP Create New Layer Mask Icon](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Create-New-Layer-Mask-Icon.jpg)

GIMP Create New Layer Mask Icon

To demonstrate these other options, I’ll first hide my Model in Red Chair layer using the show/hide icon in the layers panel (red arrow). Then, I’ll click on the Zion image layer (green arrow), and will click the icon at the bottom of the Layers panel (blue arrow) that allows me to add a new layer to the image layer (this is simply another option for adding a layer mask – it performs the same task as right clicking, then going to “Add Layer Mask” within the Layer Context Menu).
#### White (full opacity)

![GIMP Layer Mask Initialize to White Full Opacity](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Layer-Mask-Initialize-to-White-Full-Opacity.jpg)

GIMP Layer Mask Initialize to White Full Opacity

In the Add Layer Mask Dialogue, we can now analyze the other options available under “Initialize Layer Mask to:.” We already know that the “White (full opacity)” will create a fully opaque image (see the beginning of this tutorial for more information).
#### Black (full transparency)

![Add Layer Mask Dialogue Black Full Transparency](https://daviesmediadesign.com/wp-content/uploads/2019/08/Add-Layer-Mask-Dialogue-Black-Full-Transparency.jpg)

Add Layer Mask Dialogue Black Full Transparency

The next option, “Black (full transparency)” will create a fully transparent image.

![GIMP Layer Mask Tutorial Black Full Transparency](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Layer-Mask-Tutorial-Black-Full-Transparency.jpg)

GIMP Layer Mask Tutorial Black Full Transparency

So, when I choose this option and click “Add,” the Zion image layer will now be completely hidden, revealing the gray checkerboard background (which represents transparency in GIMP – denoted by the blue arrow). You will also see that the Zion layer has a black layer mask thumbnail in the Layers panel (red arrow).

![Painting White on a Black Layer Mask in GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Painting-White-on-a-Black-Layer-Mask-in-GIMP.jpg)

Painting White on a Black Layer Mask in GIMP

The image will remain fully transparent until I paint white on my layer mask (which I have done in the image above). Any area where I paint white will reveal those pixels in the image, and those white areas will also display in the layer mask thumbnail.

I’ll hit ctrl+z twice to undo the last two steps, going back to my original Zion image before I added my layer mask.
#### Layer’s Alpha Channel

![Initialize Layer Mask to Layers Alpha Channel GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Initialize-Layer-Mask-to-Layers-Alpha-Channel-GIMP.jpg)

Initialize Layer Mask to Layers Alpha Channel GIMP

The next option in the “Add Layer Mask” dialogue box is to Initialize the Layer Mask to the “Layer’s Alpha Channel.”

The alpha channel, if you’ll remember from the previous GIMP Layers article on Layer Transparency, is the channel that represents the layer’s transparency. So, with this layer mask option, a layer mask will be created based on any transparent areas of the image. If there are no transparent areas in the image, then the layer mask will simply be white.

![Erasing Image to Reveal Transparency in GIMP 2 10](https://daviesmediadesign.com/wp-content/uploads/2019/08/Erasing-Image-to-Reveal-Transparency-in-GIMP-2-10.jpg)

Erasing Image to Reveal Transparency in GIMP 2 10

On the other hand, I can close my Add Layer Mask dialogue box (by hitting the Cancel button or the “x” in the top right corner), grab my eraser tool (red arrow), and erase a random part of the image. You can see where I erased because the gray checkerboard background is now revealed, showing the transparent area (green arrow).

![Layers Alpha Channel with Erased Area GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Layers-Alpha-Channel-with-Erased-Area-GIMP.jpg)

Layers Alpha Channel with Erased Area GIMP

Now, if I click the “Add Layer Mask” icon again (red arrow), select the “Layer’s Alpha Channel” option, and click “Add” (green arrow)…

![Layers Alpha Channel Layer Mask GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Layers-Alpha-Channel-Layer-Mask-GIMP.jpg)

Layers Alpha Channel Layer Mask GIMP

…the area I erased will now show up as black on the layer mask (red arrow). Everything else that I did not erase (the opaque portions of my image layer) will still show up as white.

The main issue with this feature is that the area you erased remains on the image, AND a layer mask is created. So, even though there is a layer mask now, it is not non-destructive because you still have the erasing that occurred directly on the layer.

Luckily, the next option in the Add Layer Mask dialogue fixes this problem.

I’ll hit ctrl+z to undo adding this layer mask.

#### Transfer Layer’s Alpha Channel

![Transfer Layers Alpha Channel Layer Mask GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Transfer-Layers-Alpha-Channel-Layer-Mask-GIMP.jpg)

Transfer Layers Alpha Channel Layer Mask GIMP

The next option in the Add Layer Mask dialogue box is to Initialize Layer Mask to “Transfer Layer’s Alpha Channel.” This option performs the same action as the “Layer’s Alpha Channel” option, except it transfers any areas of opacity from the layer to the layer mask. In other words, it does not keep the transparent areas on the layer mask AND create a layer mask from these areas. So, this option is more non-destructive. I’ll click the “Add” button to add this layer mask type to my image.

![Transfer Alpha Channel Layer Mask Example](https://daviesmediadesign.com/wp-content/uploads/2019/08/Transfer-Alpha-Channel-Layer-Mask-Example.jpg)

Transfer Alpha Channel Layer Mask Example

If I have the “Disable Layer Mask” option checked in my Layer Context Menu (green arrow), you’ll see that the image now looks normal again (red arrow – this is the area where the erased portion was) – it does not have the transparency area we created with the eraser tool. That area now only exists on the layer mask.

I’ll right click on the layer and go to “Delete Layer Mask” from the Layer Context Menu to delete the layer mask. Since the transparency area was lifted or removed from our Zion image, this returns the image back to its original state (before we applied the eraser tool).
#### Selection

![Add a Mask to Selection in GIMP 2019 Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/Add-a-Mask-to-Selection-in-GIMP-2019-Tutorial.jpg)

Add a Mask to Selection in GIMP 2019 Tutorial

Next up we have the Initialize Layer Mask to “Selection” option in the Add Layer Mask dialogue.

If you do not currently have a selection area on your composition, clicking the “Add” button with this option selected will simply create a black layer mask. This, of course, will make your layer transparent.

![GIMP Mask to Selection Layer Mask Selection](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Mask-to-Selection-Layer-Mask-Selection.jpg)

GIMP Mask to Selection Layer Mask Selection

However, let’s say I right click on my Model in Red Chair layer and go to “Mask to Selection” (red arrow). This will create a selection area around the layer mask on this layer (as mentioned earlier in this article – denoted by the green arrow).

![Add a Mask to Selection in GIMP 2019 Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/Add-a-Mask-to-Selection-in-GIMP-2019-Tutorial.jpg)

Add a Mask to Selection in GIMP 2019 Tutorial

Now that I have a selection on my composition, I can click on my Zion layer once again and click on the “Add Layer Mask” icon. Now, I’ll once again choose the “Selection” option.

![Selection Layer Mask GIMP 2 10 12 Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/Selection-Layer-Mask-GIMP-2-10-12-Tutorial.jpg)

Selection Layer Mask GIMP 2 10 12 Tutorial

When I click “Add,” a new layer mask is created with everything inside the selection area showing up as white (full opacity) and everything outside the selection area showing up as black (full transparency). You can see this reflected in the layer mask thumbnail (red arrow). In other words, everything inside of the selection area remains visible (green arrow).

I’ll hit ctrl+shift+a to deselect my selection area. My Zion photo now has essentially the same layer mask as my Model in Red Chair layer.

I’ll right click and go to “Delete Layer Mask” to delete this newly created layer mask.

#### Grayscale Copy of Layer

![Grayscale Copy of Layer GIMP Layer Mask](https://daviesmediadesign.com/wp-content/uploads/2019/08/Grayscale-Copy-of-Layer-GIMP-Layer-Mask.jpg)

Grayscale Copy of Layer GIMP Layer Mask

The next option is to create a layer mask based on a black and white (also called “Graysacle”) copy of our active layer.

![Adding Layer Mask GIMP Grayscale Copy of Layer](https://daviesmediadesign.com/wp-content/uploads/2019/08/Adding-Layer-Mask-GIMP-Grayscale-Copy-of-Layer.jpg)

Adding Layer Mask GIMP Grayscale Copy of Layer

When I select the “Grayscale Copy of Layer” option, and click “Add,” a new layer mask will be created that is simply a black and white version of our main image layer. You can see this by looking at the newly create layer mask thumbnail in the Layers panel (red arrow). This grayscale copy layer mask creates some interesting effects because of the basic principle of how black, white, and gray work on a layer mask. Any black parts of the black and white image layer mask now become transparent on our image layer, gray becomes semi-transparent, and white remains fully opaque.

![Black and White Image Layer Mask in GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Black-and-White-Image-Layer-Mask-in-GIMP.jpg)

Black and White Image Layer Mask in GIMP

I can also right click on my layer mask and go to “Show Layer Mask” to get a better look at the grayscale copy layer mask. As you can see in the photo above, the layer mask is simply a black and white version of our original image.

I’ll hit ctrl+z to undo the “Show Layer Mask” action.

I’ll right click on the layer mask and go to “Delete Layer Mask” to get ready for the last option.
#### Channel

![Initialize Layer Mask to Channel in GIMP](https://daviesmediadesign.com/wp-content/uploads/2019/08/Initialize-Layer-Mask-to-Channel-in-GIMP.jpg)

Initialize Layer Mask to Channel in GIMP

The last layer mask option is the “Channel” option. In order for this option to work, you have to create a custom channel in the “Channels” tab. Right now, I do not have a custom channel, so the dropdown box is blank.

![Creating a Custom Channel in GIMP Channels Tab](https://daviesmediadesign.com/wp-content/uploads/2019/08/Creating-a-Custom-Channel-in-GIMP-Channels-Tab.jpg)

Creating a Custom Channel in GIMP Channels Tab

Let’s say, for example, while in the Channels tab (blue arrow) that I click and drag the Red channel into the custom channels area (click and drag your mouse following the red arrows in the photo above). I will now have a duplicate red channel for my composition.

![Add Layer Mask Using Custom Channel in GIMP Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/Add-Layer-Mask-Using-Custom-Channel-in-GIMP-Tutorial.jpg)

Add Layer Mask Using Custom Channel in GIMP Tutorial

Now, I’ll go back to the Layers panel, click on the Zion image layer, and click the icon to create a new layer mask (red arrow).

If I click the Initialize Layer Mask to “Channel” option, you will see that our custom red channel is now listed here (Red Channel Copy – denoted by the blue arrow).

![Red Channel Layer Mask GIMP 2019](https://daviesmediadesign.com/wp-content/uploads/2019/08/Red-Channel-Layer-Mask-GIMP-2019.jpg)

Red Channel Layer Mask GIMP 2019

I’ll click “Add,” and my Red Channel Copy channel has now been added to my layer as a layer mask. Since channels are represented in grayscale, the effect is similar to the “Grayscale copy of layer” option as you can see in the layer mask thumbnail (red arrow). However, there are some slight differences since only the red colors and their shades are represented here.

I’ll right click and go to “Delete Layer Mask” to return to my original image.

#### Invert Checkbox

![GIMP White Layer Mask Invert Option](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-White-Layer-Mask-Invert-Option.jpg)

GIMP White Layer Mask Invert Option

Below all of the “Initialize Layer Mask to:” options is a check box that allows you to invert any of the effects created by the selected option.

For example, if I have the Invert box checked (outlined in green in the photo above), and I click the “White (full opacity)” option (red arrow), my layer mask will now show up as black – the opposite or invert of white.

I’ll hit ctrl+z to undo this action.

![GIMP Mask to Selection Layer Mask Selection](https://daviesmediadesign.com/wp-content/uploads/2019/08/GIMP-Mask-to-Selection-Layer-Mask-Selection-1.jpg)

GIMP Mask to Selection Layer Mask Selection

Another example is if I right click on the Model in Red Chair layer, then go to “Mask to Selection” (red arrow). This will once again produce a selection area around the layer mask on this layer.

![Invert Selection Layer Mask GIMP Tutorial](https://daviesmediadesign.com/wp-content/uploads/2019/08/Invert-Selection-Layer-Mask-GIMP-Tutorial.jpg)

Invert Selection Layer Mask GIMP Tutorial

Next, I’ll click on the Zion layer and click the Add Layer Mask icon. I’ll choose “Selection,” and keep the Invert box checked.

![Selection Layer Mask Inverted GIMP 2 10 12](https://daviesmediadesign.com/wp-content/uploads/2019/08/Selection-Layer-Mask-Inverted-GIMP-2-10-12.jpg)

Selection Layer Mask Inverted GIMP 2 10 12

When I click “Add,” everything outside my selection area is now white, and everything inside the selection area is black (as you can see in the layer mask thumbnail – red arrow in the photo above). This is the opposite effect for the normal “Selection” option that I demonstrated earlier in this tutorial. The top of the image is visible, and the bottom portion is transparent.

I’ll hit ctrl+z to undo this action, and will now unhide my Model in Red Chair layer.

As you can see, there are tons of effects you can create using layer masks. I only brushed the surface of what this feature can accomplish in GIMP, but I use it all the time when working with compositions that have multiple images or layers. If you can master layer masks, you are truly on your way to mastering GIMP.

That’s it for this tutorial! Next up in this GIMP Layers series, I’ll show you how to work with [Layer Groups](https://daviesmediadesign.com/gimp-layer-groups-everything-you-need-to-know/). If you liked this tutorial, you can check out any of my [GIMP Help articles](https://daviesmediadesign.com/gimp-help-articles/) or [GIMP Video Tutorials](https://daviesmediadesign.com/tutorials/).


