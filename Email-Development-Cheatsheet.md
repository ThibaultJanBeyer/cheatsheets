[back to overwiev](/../..)

# HTML Email Development Cheatsheet

## Sources:
- https://frontendmasters.com/courses/html-email-v2/background-images/
- https://github.com/rodriguezcommaj/frontendmasters
- https://codepen.io/collection/21b7ddd5c58aafc450c35c46e7cba9b5?grid_type=list

## Table of Contents  
- [Basics](#basics)  
- - [Basic HTML Email](#basic-html-email)
- - [CSS Resets](#css-resets)
- - [Email friendly HTML](#email-friendly-html)
- - [Email friendly CSS](#email-friendly-css)
- [Links & Buttons](#links-&-buttons)
- [Images](#images)
- - [Responsive Images](#responsive-images)
- - [Background Images](#background-images)
- [Accessibility](#accessibility)
- [Layouts](#layouts)
- [Mobile Emails](#mobile-emails)
- [Interactivity](#interactivity)

## Basics

- Marketing emails should have unsubscribe links
- Use email testing tools like [email on acid](https://www.emailonacid.com/) or [litmus](https://www.litmus.com/extension/)
- Use [Putsmail](https://putsmail.com) to enter html and it will send it out as an email
- Use [Can I Email](https://caniemail.com) or [Campaign Monitor](https://campaignmonitor.com) or [Fresh Inbox](https://freshinbox.com) to see what's supported in which email clients. just like `caniuse.com`.
- You can use tools to help you build emails like `Litmus builder`, `mjml.io`, `Inky` (from foundation.zurb), `Maizzle` to help you.
- Support channels: `email.geeks.chat`, `litmus.com/community`, `thebetter.email`

### Basic HTML Email

The basic HTML for Emails looks pretty much like the basic HTML for a website

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>Document</title>
  <style type="text/css">
    
  </style>
</head>
<body id="body" style="margin: 0 !important; padding: 0 !important;">
  
</body>
</html>
```

- `lang` make sure to pass the proper lang for screen readers to know in which language tho read the text
- `body style=` we want to reset the margins and paddings to make sure that the email client does not add something weird to our emails

### CSS Resets

- Email clients add CSS to parts of the email on their own. I.e. highlighting times, phones, dates etc (to add to the calendar or call)
- We want to keep the behavior but improve it's styling

```
/* CLIENT-SPECIFIC STYLES */
body, table, td, a { -webkit-text-size-adjust: 100%; -ms-text-size-adjust: 100%; }
table, td { mso-table-lspace: 0pt; mso-table-rspace: 0pt; }
img { -ms-interpolation-mode: bicubic; }

/* RESET STYLES */
img { border: 0; height: auto; line-height: 100%; outline: none; text-decoration: none; }
table { border-collapse: collapse !important; }
body { height: 100% !important; margin: 0 !important; padding: 0 !important; width: 100% !important; }

/* iOS BLUE LINKS */
a[x-apple-data-detectors] {
    color: inherit !important;
    text-decoration: none !important;
    font-size: inherit !important;
    font-family: inherit !important;
    font-weight: inherit !important;
    line-height: inherit !important;
}

/* GMAIL BLUE LINKS */
u + #body a {
    color: inherit;
    text-decoration: none;
    font-size: inherit;
    font-family: inherit;
    font-weight: inherit;
    line-height: inherit;
}

/* SAMSUNG MAIL BLUE LINKS */
#MessageViewBody a {
    color: inherit;
    text-decoration: none;
    font-size: inherit;
    font-family: inherit;
    font-weight: inherit;
    line-height: inherit;
}
```

Taken from [Rodrigues Commajs example](https://github.com/rodriguezcommaj/frontendmasters/blob/master/Code%20Examples/Background-Image.html)

- Text size 100%: to make sure that they don't adjust the font-size settings (i.e. ios automatically bumps up small font-sizes to minimum 14px)
- mso-table: makes sure that there is no spacing around tables
- interpolation-mode: improves quality of imagery
- style resets: make sure the doc. renders as intended
- Link overrides: to make sure that the links that are automatically added by the email client don't get custom styling

### Email friendly HTML

- Use these for most things: `div`, `span`, `h1`-`h6`, `p`, `strong`, `em`, `img` (simple html tags)
- Most structuring is done with `table`s
- All URLs should be absolute URIs that also contain the protocol, not relative. So `https://google.com/image.jpg` instead of `/image.jpg` or `google.com/image.jpg`.
- `<center></center>` is completely deprecated but can be used to centering parts in outlook.

### Email friendly CSS

- Don’t use Linked Stylesheets. A lot of email clients will remove those. Use embedded styles or even better inline styles.
- For text: `color`, `font-family`, `font-size`, `font-style`, `font-weight`, `line-height`, `text-align`.
- For block-level: `margin`, `padding`, `width`, `max-width` (might not always work), `border`.
- Make sure to search online to see what CSS properties are supported in what email client.
- Use pixel sizing `px` instead of relative units etc.
- You can use shorthands. i,e, `margin: 40px 0;` or `margin: 0 auto` to center the content

## Links & Buttons

- Don't use images for buttons
- Use descriptive links (not this "click here", use "read the article" for example)
- Embrace link conventions (i.e. blue and underlined text for links)
- Don't just rely on color. Don't underline things that are not links (might frustrate the user into thinking it's a link)

### Bulletproof buttons

- from https://buttons.cm/ (most reliable):
```html
<div><!--[if mso]>
  <v:roundrect xmlns:v="urn:schemas-microsoft-com:vml" xmlns:w="urn:schemas-microsoft-com:office:word" href="http://" style="height:40px;v-text-anchor:middle;width:200px;" arcsize="10%" strokecolor="#1e3650" fill="t">
    <v:fill type="tile" src="https://i.imgur.com/0xPEf.gif" color="#556270" />
    <w:anchorlock/>
    <center style="color:#ffffff;font-family:sans-serif;font-size:13px;font-weight:bold;">Show me the button!</center>
  </v:roundrect>
<![endif]--><a href="http://"
style="background-color:#556270;background-image:url(https://i.imgur.com/0xPEf.gif);border:1px solid #1e3650;border-radius:4px;color:#ffffff;display:inline-block;font-family:sans-serif;font-size:13px;font-weight:bold;line-height:40px;text-align:center;text-decoration:none;width:200px;-webkit-text-size-adjust:none;mso-hide:all;">Show me the button!</a></div>
```

- Padding based:
```html
<table width="100%" border="0" cellspacing="0" cellpadding="0">
    <tr>
        <td align="center" style="padding: 60px;">
            <table border="0" cellspacing="0" cellpadding="0">
                <tr>
                    <td brcolor="#229efd" style="padding: 12px 18px 12px 18px; border-radius: 3px" align="center">
                        <a href="…" target="_blank" style="font-size: 18px; text-decoration: none; display: inline-block;">Button</a>
                    </td>
                </tr>
            </table>
        </td>
    </tr>
</table>
```

- Border based
```html
<table width="100%" border="0" cellspacing="0" cellpadding="0">
    <tr>
        <td align="center" style="padding: 60px;">
            <table border="0" cellspacing="0" cellpadding="0">
                <tr>
                    <td>
                        <a href="…" target="_blank" style="font-size: 18px; text-decoration: none; display: inline-block; color: #ffffff; background-color: #229efd; border-top: 12px solid #229efd; border-bottom: 12px solid #229efd; border-right: 18px solid #229efd;border-left: 18px solid #229efd;">Button</a>
                    </td>
                </tr>
            </table>
        </td>
    </tr>
</table>
```

## Images

- Make them responsive by default
- Use alt text describing the image
- Stick to standards: jpg, png, gif

### Responsive Images

- Set a fixed width for outlook `<img … width="600" border="0" … >`
- `display: block; max-width: 100%; min-width: 100px; width: 100%;` to make them adjust across screen sizes

i.e.
```html
<img width="600" border="0" style="display: block; max-width: 100%; min-width: 100px; width: 100%" src="img/image.jpg" alt="puppy licking ice-cream">
```

### Background Images

- Most reliable on table cells (td)
- Use both: HTML attributes and inline CSS

i.e.
```html
<td background="image/bg.jpg" bgcolor="#229efd" style="background: #229efd url('image/bg.jpg')">
``` 

## Accessibility

- Left align longer text (center align only short text/headings etc.)
- Keep color contrast high
- Create a strong visual hierarchy
- Focus on readability
- Keep layouts simple and usable
- Use real text instead of images
- Keep tables quiet using `role="presentation"`
- Include text alternatives for images
- Include the language of an email `lang="de"`

### Testing

- Close your eyes and use a real screen reader: NVDA, VoiceOver, JAWS, Browser Extensions (NoCoffeee Vision Simulator, silktide, toadly, lighthouse), Litmus accessibility checker.

## Layouts

- Think in modules
- Use `role="presentation"` on any table
- Don't use table headers, body, footer
- Each component lives in their own rows/tables
- Override defaults using HTML attributes
- Most styles should be included in table tags

Boilerplate: 
```html
<!-- Outer Fluid Container Table -->
<table border="0" cellpadding="0" cellspacing="0" role="presentation" width="100%">
    <tr>
        <td>
            <!-- Inner Fixed Container Table -->
            <table border="0" cellpadding="0" cellspacing="0" role="presentation" width="600">
                <!-- Each TR is it's own email "module", i.e. logo, headline, hero image, etc. -->
                <tr>
                    <td>
                        <!-- logo -->
                    </td>
                </tr>
                <tr>
                    <td>
                        <!-- headline -->
                    </td>
                </tr>
            </table>

        </td>
    </tr>
</table>
```

- Basic structure is: Fluid table => Fixed table => Fluid table => Fixed table

## Mobile Emails

- For emails it makes sense to design in desktop first because otherwise you'd get the mobile version on the desktop as the queries don't always work.
- You can use media queries
- Still work with tables but make those responsive

i.e.
```css
@media screen and (max-width: 600px) {
    .mobile { width: 100% !important }
    .block { display: block !important; width: 100% !important; }
}
```

And add them to the tables i.e. to make them stack.

### Hybrid/Spongy Coding

- Fluid by default
- max-width


- MSO ghost tables:
```
<!--[if (gte mso 9)|(IE)]>
<table cellspacing="0" cellpadding="0" border="0" width="600" align="center" role="presentation"><tr><td>
<![endif]>

…content here…

<!--[if (gte mso 9)|(IE)]>
</td></tr></table>
<![endif]>
```
- This will wrap the whole content in tables for Outlook
- `gte` = greater than or equal, `gt` = greater than, `lte` = less than equal to, `lt` = less than.
- `9` = Outlook 2000, `10` = 2002, …, `15` = Outlook 2013

## Interactivity

- `:hover` pseudo selector works (only when included in the header)
- gifs
- movableink (using images that are generated when requested via GET requests. i.e. countdown timer etc.)
- “The checkbox hack” to create state (by using input labels to toggle checkboxes that changes display with sibling selectors to create interactivity):

```html
<input type="radio" class="slide-input" name="slides" id="slide1" checked>
<input type="radio" class="slide-input" name="slides" id="slide2">
<input type="radio" class="slide-input" name="slides" id="slide3">
<label for="slide1" class="slide-label">Slide 1</label>
<label for="slide2" class="slide-label">Slide 2</label>
<label for="slide3" class="slide-label">Slide 3</label>
<div class="slide-content" id="content1">Content 1</div>
<div class="slide-content" id="content2">Content 2</div>
<div class="slide-content" id="content3">Content 3</div>
```

```css
.slide-input { display: 'none' }
.slide-label { cursor: 'pointer' }
.slide-input ~ .slide-content { display: 'none' }
#slide1:checked ~ #content1 { display: 'block' }
#slide2:checked ~ #content2 { display: 'block' }
#slide3:checked ~ #content3 { display: 'block' }
```
