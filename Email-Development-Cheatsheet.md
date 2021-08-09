[back to overwiev](/../..)

# HTML Email Development Cheatsheet

## Table of Contents  
- [Basics](#basics)  
- - [Basic HTML Email](#basic-html-email)
- - [CSS Resets](#css-resets)
- - [Email friendly HTML](#email-friendly-html)
- - [Email friendly CSS](#email-friendly-css)

## Basics

- Marketing emails should have unsubscribe links
- Use email testing tools like [email on acid](https://www.emailonacid.com/) or [litmus](https://www.litmus.com/extension/)

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
