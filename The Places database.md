# The Places database
  The Places database                    

-   [Skip to main content](#content)

[![](http://udn.realityripple.com/static/img/web-docs-black.png)
](/)

-   Technologies
    -   [Technologies Overview](/docs/Web)
    -   [XUL](/docs/Archive/Mozilla/XUL)
    -   [HTML](/docs/Web/HTML)
    -   [CSS](/docs/Web/CSS)
    -   [JavaScript](/docs/Web/JavaScript)
    -   [XPCOM](/docs/Mozilla/Tech/XPCOM)
    -   [Graphics](/docs/Web/Guide/Graphics)
    -   [HTTP](/docs/Web/HTTP)
    -   [APIs / DOM](/docs/Web/API)
    -   [MathML](/docs/Web/MathML)
-   References & Guides
    -   [Learn web development](/docs/Learn)
    -   [Tutorials](/docs/Web/Tutorials)
    -   [References](/docs/Web/Reference)
    -   [Developer Guides](/docs/Web/Guide)
    -   [Accessibility](/docs/Web/Accessibility)
    -   [Game development](/docs/Games)
    -   [...more docs](/docs/Web)
-   Additional Resources

    -   [Archive](/docs/Archive)
    -   [UXP Homepage 🌐](http://thereisonlyxul.org/)
    -   [Pale Moon on GitHub 🌐](//github.com/RealityRipple/Pale-Moon/)
    -   [UXP on GitHub 🌐](//github.com/RealityRipple/UXP/)
    -   [Pale Moon Forum 🌐](//forum.palemoon.org/)
    -   [Support Pale Moon 🌐](//www.palemoon.org/donations.shtml)
    -   [Support the Backup](//realityripple.com/donate.php?itm=UDN)[Additional Details](https://realityripple.com/fossil.php)

    search

# The Places database

1.  [See Mozilla](/docs/Mozilla)
2.  [See Mozilla technologies](/docs/Mozilla/Tech)
3.  [See Places](/docs/Mozilla/Tech/Places)
4.  [The Places database](/docs/Mozilla/Tech/Places/Database)

This document provides a high-level overview of the overall database design of the [Places](/docs/Mozilla/Tech/Places) system. Places is designed to be a complete replacement for the Firefox bookmarks and history systems using [Storage.](/docs/Mozilla/Tech/XPCOM/Storage)

The places schema looks like so:

![](http://udn.realityripple.com/static/external/a0/035c1a0c16848a58ed746e488654c389a5f2eae17fe91e374fb5bbdd56a805.png)

### Core URL table

-   **moz_places**: This is the main table of URIs and is managed by the [history service](/docs/Mozilla/Tech/Places/Using_the_Places_history_service) (see also [History service design](/docs/Mozilla/Tech/Places/History_Service_Design)). Any time a Places component wants to reference a URL, whether visited or not, it refers to this table. Each entry has an optional reference to the `moz_favicon` table to identify the favicon of the page. No two entries may have the same value in the url column.
-   **moz_hosts**: One entry in this table is created each time you visit a new host. It contains the host url, frequency of access, if typed or not, and it's prefix (https&#x3A;//, ftp://, etc).

### History tables

-   **moz_historyvisits**: One entry in this table is created each time you visit a page. It contains the date, referrer, and other information specific to that visit. It contains a reference to the `moz_places` table which contains the URL and other global statistics.

See [History service design](/docs/Mozilla/Tech/Places/History_Service_Design) for more information.

### Bookmarks Tables

-   **moz_bookmarks**: This table contains bookmarks, folders, separators and tags, and defines the hierarchy. The hierarchy is defined via the parent column, which points to the `moz_bookmarks` record which is the parent. The position column numbers each of the peers beneath a given parent starting with 0 and incrementing higher with each addition. The fk column provides the id number of the corresponding record in `moz_places`.

-   **moz_bookmarks_roots**: Lists special folders that are the root folders of certain content types in the bookmarks system. The records in this table are mapped to records in the `moz_bookmarks` table. Bookmarks, folders and separators are descendants of the bookmarks root, while tags and tagged URIs are descendants of the tag root.

-   **moz_keywords**: This table is a unique list of keywords. The `moz_bookmarks` table has a `keyword_id` column that maps to a record in `moz_keywords` table.

See [Manipulating bookmarks using Places](/docs/Mozilla/Tech/Places/Manipulating_bookmarks_using_Places) for more information.

### Annotation Tables

-   **moz_anno_attributes**: Contains the names of all the annotations in the system and a name ID. There should be relatively few unique names.

-   **moz_annos**: Contains the values of page annotations. It maps a page (reference to `moz_places`) and an annotation name (reference to `moz_anno_attributes`) to the value of the annotation.

-   **moz_items_annos**: Contains the values of bookmark item annotations. It maps a bookmark, folder or separator (reference to `moz_bookmarks`) and an annotation name (reference to `moz_anno_attributes`) to the value of the annotation.

### Favicon table

-   **moz_favicons**: This contains a list of unique favicon URIs and data. One or more pages in `moz_places` refer to each entry. When no pages reference a favicon, the icon entry will be deleted.
    -   If the mime type of the image is image/png, the data blob must be reencoded from base16 (the format in which it is stored) to base64 in order to display correctly.

### Expiration

Expiration is handled in `[toolkit/components/places/nsPlacesExpiration.js](//github.com/RealityRipple/UXP/blob/master/toolkit/components/places/nsPlacesExpiration.js)`. This algorithm determines the lifetime of all objects in the Places system.

Periodically at runtime, the following happens to expire pages:

-   Expire visits that are older than the history expiration threshold.
-   Delete the history entries that were referenced by the expired visits and not referenced by any non-expired visit or bookmark.
-   Delete favicons for those expiring history entries that are not referenced by any other history entry.
-   Delete annotations that belong to the expiring pages.

At shutdown time, an extra step is performed in case there are any orphans. There are several ways for orphans to be created, including calling `markPageAsTyped` and then never visiting the page. Extensions might also set favicons for pages that have never been visited.

-   Delete any history entries that have no visits, are not bookmarked, and are not place: URIs.
-   Delete any favicons that are not referenced by any history entries.

See [Places Expiration](/docs/Mozilla/Tech/Places/Places_Expiration) for more information.

[MDN Web Docs](/)

© 2005-2020 [Mozilla and individual contributors](https://developer.mozilla.org/docs/MDN/About#Copyrights_and_licenses). Content is available under [CC BY-SA-2.5](https://realityripple.com/license.php?l=CC-BY-SA-2.5).  
Backup generated August, 2020 by [RealityRipple Software](https://realityripple.com).

-   ~Terms~
-   [Privacy](https://realityripple.com/privacy.php)
-   ~Cookies~
