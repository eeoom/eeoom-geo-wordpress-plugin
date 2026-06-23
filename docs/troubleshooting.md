# Troubleshooting

This troubleshooting guide helps diagnose common issues with the **eeoom GEO WordPress plugin**.

Use this document when checks do not run, reports look incomplete, structured data is missing, pages are not detected correctly or recommendations do not match what you expect.

## Basic checks

Start with the simple checks first.

Make sure that:

- the plugin is activated
- you are logged in with a role that is allowed to use eeoom GEO
- the page you want to analyze is published
- the page is publicly accessible
- the page is not password-protected
- the website uses HTTPS
- WordPress permalinks are working
- the WordPress REST API is available
- caching is not serving an outdated version of the page

Many issues are caused by access, caching or conflicting plugins rather than the eeoom GEO plugin itself.

## The plugin menu is not visible

If the **eeoom GEO** menu does not appear in the WordPress admin area, check the following:

- Is the plugin activated under **Plugins**?
- Are you logged in as an administrator?
- Does your user role have permission to access eeoom GEO?
- Is another plugin hiding admin menu items?
- Is there a PHP fatal error preventing the menu from loading?
- Is the plugin installed in the correct WordPress installation?

If the plugin is active but the menu is still missing, check the WordPress debug log for PHP errors.

## A check does not start

If a visibility, structured data or page-level check does not start, possible causes include:

- missing permission for the current user role
- expired WordPress nonce
- JavaScript error in the admin area
- blocked admin AJAX request
- blocked REST API request
- security plugin interference
- caching plugin interference
- server timeout
- missing API configuration
- network connection problem

Try reloading the WordPress admin page and starting the check again.

If the problem continues, open the browser developer tools and check the **Console** and **Network** tabs for errors.

## A check starts but never finishes

If a check starts but stays in a loading state, this can be caused by:

- server timeout
- long response time from the analyzed page
- blocked outbound HTTP request
- blocked REST API polling
- insufficient PHP memory
- fatal PHP error during analysis
- external API timeout
- caching or security plugin interference

Recommended checks:

1. Open the browser developer tools.
2. Check whether repeated polling requests are successful.
3. Check the WordPress debug log.
4. Check the server error log.
5. Temporarily disable aggressive caching or security rules.
6. Try analyzing a simple public page first.

## The analyzed page is not found

If eeoom GEO cannot find or analyze a page, check whether the page is:

- published
- public
- accessible without login
- not password-protected
- not redirected unexpectedly
- not blocked by `robots.txt`
- not marked as `noindex`
- not returning a `404`, `403` or `500` status
- using the correct canonical URL

Also check whether the URL contains unusual query parameters or redirects to a different language, domain or path.

## Drafts or private content appear in candidates

eeoom GEO should usually focus on public-facing website content.

If drafts, private posts or internal content appear in analysis candidates, check whether the query or post selection logic only includes:

- `publish` post status
- public post types
- public pages
- public posts
- relevant custom post types

Drafts, private posts, revisions, attachments and internal admin pages should generally be excluded from public AI search visibility checks.

## Already added pages are not marked correctly

If pages that were already added are not marked as already added, possible causes include:

- comparison uses the wrong post ID
- comparison uses URL instead of post ID, or the other way around
- URLs are stored with different trailing slash behavior
- URLs are stored with different domains or protocols
- canonical URLs differ from WordPress permalinks
- cache contains outdated candidate data
- duplicate posts have similar titles or slugs
- the saved priority page list was not refreshed after adding a page

Recommended checks:

- compare by stable WordPress post ID when possible
- normalize URLs before comparing them
- clear cached candidate results
- reload the priority page list after adding a page
- verify that the saved page record contains the expected post ID and URL

## Clicking a button opens a blank admin page

If clicking a button such as **Add Monitor**, **Add Priority Page** or **Run Check** opens a blank WordPress admin page, possible causes include:

- missing action handler
- wrong admin URL
- missing redirect after processing an action
- expired or invalid nonce
- missing permission check result
- PHP fatal error
- output is generated before a redirect
- the action parameter is not recognized
- the page callback does not render content for that state

Recommended checks:

- verify the admin URL contains the expected `page` parameter
- verify the action parameter is correct
- verify the nonce is created and checked correctly
- verify the user capability check passes
- verify the handler redirects back to a valid admin page
- check the PHP error log
- enable WordPress debugging temporarily

A WordPress admin action should usually process the request and then redirect back to a visible admin page with a success or error notice.

## Structured data is missing

If structured data is missing from a page, check the following:

- Is an SEO or schema plugin responsible for outputting JSON-LD?
- Is the theme removing schema output?
- Is the page template missing required hooks such as `wp_head()`?
- Is the page rendered differently for logged-in and logged-out users?
- Is the structured data only added after JavaScript execution?
- Is the page cached without the expected JSON-LD?
- Is another plugin filtering or removing schema markup?

View the public page source as a logged-out visitor and search for:

```html
application/ld+json
