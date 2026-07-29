# WordPress Elite Hunter — Dense Reference

>90–96% of paid WordPress bugs are in plugins/themes. Core is hard; plugins are soft.

## 1. Immediate Fingerprinting

- Core version: generator meta, readme.html, /wp-includes/version.php, JS/CSS query strings
- Plugins: /wp-content/plugins/, readme.txt, main PHP headers, asset paths, REST namespaces
- Themes: style.css header, /wp-content/themes/
- Multisite? Look for /wp-signup.php, network admin, blogs.dir or sites/ structure
- Cross-check every version against Wordfence Intelligence + Patchstack + WPScan before deep work

## 2. Capability & Nonce Failures (Highest Volume Class)

Common broken patterns:
- add_action('wp_ajax_xxx' or 'wp_ajax_nopriv_xxx') with no current_user_can()
- register_rest_route() with missing permission_callback or permission_callback => '__return_true'
- check_ajax_referer() present but no capability check (nonce ≠ authorization)
- Capability checked is too broad (e.g. 'read' or 'edit_posts' for admin actions)
- Object-level checks missing even when function-level capability exists

Test procedure:
1. Create low-priv account (Subscriber or Customer)
2. Capture all AJAX and REST calls while using the app as higher priv if possible
3. Replay every state-changing request with low-priv session
4. Also try unauthenticated (nopriv) versions
5. Watch for successful state change, data return, or privilege gain

## 3. REST API Systematic Checks

For every route:
- permission_callback exists and is correct?
- Does it enforce object ownership (current user owns the post/user/media/order)?
- Can I change ID and read/write another user’s object? (IDOR/BOLA)
- Mass assignment: extra fields in JSON body accepted?
- Drafts, private posts, revisions, password-protected content leaked?
- /wp/v2/users enumeration still open?
- Media upload allowed for low priv?
- Batch endpoint or route confusion present?

## 4. XML-RPC

- system.listMethods → see what is enabled
- system.multicall → amplified brute or multi-action
- pingback.ping → classic SSRF (internal ports, cloud metadata, file://)
- wp.uploadFile, wp.getUsersBlogs
- Even “disabled” plugins sometimes re-enable methods

## 5. Privilege Escalation Paths

- update_user_meta / wp_update_user reachable → change role
- update_option reachable → change default role, siteurl, active_plugins, etc.
- File delete of wp-config.php → forces reinstall wizard → new admin
- Theme/plugin file editor or update mechanisms
- Registration open + default role elevated
- Invitation / membership plugins with weak checks

## 6. Object Injection / Deserialization

- unserialize() or maybe_unserialize() on user-controlled data (cookies, POST, options, meta, widgets)
- PHAR deserialization via file_exists / is_file / file_get_contents with attacker path
- POP gadget chains (Monolog and others common in WP ecosystem)
- Prefer json_decode; if unserialize needed use allowed_classes => false

## 7. File Upload → RCE

- Media REST, admin-ajax upload handlers, form plugins, page builders
- Extension vs Content-Type vs magic bytes mismatch
- Double extensions, null bytes (legacy), path traversal in filename
- SVG (XSS/XXE), polyglots, Zip Slip on extractors
- Upload lands in executable location or is later included

## 8. Gutenberg / Blocks

- render_callback and render.php — look for echo of attributes without escaping
- block.json attribute types — injection into server render
- Dynamic blocks with user-controlled content
- Stored XSS in block content that executes in admin or front

## 9. WooCommerce Quick Hits

- Order IDOR (change order ID in my-account or API)
- Coupon apply race / reuse / stacking
- Negative quantity or price in cart/update
- Store API endpoints with weak authz
- Webhook endpoints with weak or missing signature
- Subscription status manipulation
- Refund or stock race

## 10. Multisite

- switch_to_blog() without capability
- Cross-site user or media access
- Network admin capability checks missing
- Site creation / domain mapping abuse
- Shared table queries missing blog_id filter

## 11. Common Chaining Patterns

Subscriber → missing capability AJAX → role=administrator
Low-priv XSS (comment/admin notice) → admin session → plugin install / theme edit → RCE
REST media IDOR + upload → webshell in uploads
XML-RPC SSRF → 169.254.169.254 → cloud creds
File delete wp-config.php → reinstall as admin
Object injection → RCE via existing gadgets

## 12. Quick Recon Checklist

- [ ] Core + all plugin/theme versions fingerprinted and checked in Wordfence/Patchstack
- [ ] /wp-json/ and all namespaces enumerated
- [ ] xmlrpc.php methods listed
- [ ] All admin-ajax actions discovered (JS, source, fuzzing)
- [ ] Uploads directory listing / sensitive files
- [ ] User enumeration vectors
- [ ] Registration / password-reset / invitation flows mapped
- [ ] Multisite indicators checked
