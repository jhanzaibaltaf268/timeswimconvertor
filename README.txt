timeswimconvertor.com — static site
===================================

DEPLOY (Cloudflare Pages)
  Upload the CONTENTS of this folder as the site root.
  Build command: none.  Output directory: /  (already built)
  Cloudflare serves "foo.html" at "/foo", which is how every link is written.
  Language pages are real folders, so /it/ serves it/index.html.
  Do not rename the .html files or the links break.

BEFORE YOU GO LIVE
  1. privacy.html — replace [DATE YOU PUBLISH] and [YOUR EMAIL ADDRESS]; keep
                    only the AdSense / analytics paragraph that is true for you
                    and delete the orange notice.
  2. contact.html — replace [YOUR EMAIL ADDRESS].
  3. sitemap.xml  — dates are the build date; refresh when you edit.

IF YOU RESTORE timeswimconverter.com
  Everything here is written for timeswimconvertor.com. To switch, run this in
  the site folder, then re-zip:

    grep -rl 'timeswimconvertor\.com' . | xargs sed -i 's/timeswimconvertor\.com/timeswimconverter\.com/g'

  _redirects already maps the old site's URLs onto these pages, so nothing
  that Bing has indexed will 404.

ENGLISH PAGES
  index.html                               /                         swim time converter
  yards-to-meters.html                     /yards-to-meters          best performer on the old site
  uk-swim-converter.html                   /uk-swim-converter        British Swimming algorithm
  swim-pace-calculator.html                /swim-pace-calculator
  fina-points-calculator.html              /fina-points-calculator
  split-calculator.html                    /split-calculator
  laps-in-a-mile-swimming.html             /laps-in-a-mile-swimming
  long-course-short-course-converter.html  /long-course-short-course-converter
  about / privacy / contact / 404

LANGUAGE PAGES  (local wording, not translations of the English copy)
  it/  es/  pt/  fr/  ja/     all cross-linked with hreflang, x-default = English
  Adding a sixth language means adding it to the hreflang block of ALL the
  others, or Google ignores the set.

SITE NAVIGATION (same on every English page)
  Header and footer both carry the full list of eight calculators, and the
  current page is marked with aria-current. That is the whole site structure:
  every page is one click from every other page, no page is more than one hop
  from the homepage.
  On screens under 920px the header nav becomes a horizontal scroll strip with
  a fade on the right edge. It is NOT hidden on mobile — if you edit the CSS,
  do not reintroduce a display:none rule there; most swim traffic is phones.
  Language pages keep their own in-page anchor nav and get About / Privacy /
  Contact in the footer, labelled in that language, pointing at the English
  pages.
  Adding a ninth calculator means adding it to the nav AND the .ftools footer
  list in all twelve English pages, plus sitemap.xml.

TWO DIFFERENT CONVERSION METHODS ARE IN USE HERE — THIS IS DELIBERATE
  Every page except uk-swim-converter.html uses the USA Swimming / Hy-Tek
  method: multiply by a factor (1.11 / 0.8925 / 1.02), then add a flat turn
  value per wall (free .8, back .6, breast 1.0, fly .7, IM .8). These do not
  change year to year.

  uk-swim-converter.html uses the British Swimming equivalent time algorithm,
  which is what Swim England clubs and the official converters use:
      LCM = [T + sqrt(T^2 + 8*TF*(d/100)^2)] / 2
      SCM = T - (TF/T)*(d/100)^2*2          both rounded to 0.1 s
  TF is per event and lives in the EV array near the bottom of that file.
  It is quadratic, so the turn advantage shrinks as the swimmer gets slower —
  that is why it disagrees with the American method, and why UK swimmers need
  the separate page. SCM and LCM only; there is no yards table.

MAINTENANCE
  FINA base times (fina-points-calculator.html) are the official World Aquatics
  figures: long course valid 1 Jan–31 Dec 2026, short course 1 Sep 2026–31 Aug
  2027. They change every year. They live in one JS object called BASE near the
  bottom of that file; update it and the "2026" wording together.

  CSS is inlined in every page for fastest first paint. The <style> block is
  identical across pages — change one and copy to the rest.

  _headers sets Cache-Control on /* rather than /*.html, because Cloudflare
  matches the request path: /yards-to-meters and /it/ have no .html in the URL
  and an extension pattern never catches them. The media blocks start with
  "! Cache-Control" to drop that value before setting their own — without the
  "!" Cloudflare joins both values with a comma and the header is meaningless.
