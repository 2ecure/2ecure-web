# 2ecure web customisations

The changes 2ecure makes to the web parts of its Matrix service, published as the corresponding source under the
AGPL-3.0 (see [LICENSE](LICENSE)). 2ecure is a private, invitation-only messenger run in Switzerland; overview of all
components and their source: https://2ecure.com/source.

2ecure is not a product of Element and is not affiliated with Element Creations Ltd or the Matrix.org Foundation.

## `mas/`: account pages (https://account.2ecure.com)

The templates of [matrix-authentication-service](https://github.com/element-hq/matrix-authentication-service) (MAS).
Each MAS release is imported unchanged in its own commit («Import matrix-authentication-service v…»), and the 2ecure
changes follow in the next commit; `mas/UPSTREAM` names the release in use (`git log -p -- mas/templates`):

- `components/2ecure_logo.html`: the 2ecure logo, inlined; used on the login and registration pages instead of the
  generic user icon.
- `pages/login.html`, `pages/index.html`: no «Create account» link (2ecure is invitation-only); a line for people
  with an invitation code instead (`components/2ecure_invite.html`).
- `components/2ecure_invite.html`: reads an invitation code from a personal link (`/register#code=…`, the fragment
  never reaches the server) and fills it in on the code step.
- `pages/register/steps/registration_token.html`: «Einladungscode» instead of «registration token» and «homeserver».
- `components/2ecure_app.html`, `pages/index.html`: the account start page leads into the app.
- `base.html`, `app.html`: «2ecure» as the page title; `base.html` also loads the invitation-code script.

MAS uses the directory as its `templates.path`.

## `element-web/`: web app (https://chat.2ecure.com)

[Element Web](https://github.com/element-hq/element-web) runs unchanged from Element's image; 2ecure adds:

- `config.json`: the configuration merged into Element Web's `config.json` (brand, links, help pages, the 2ecure
  server only).
- `modules/2ecure/`: `index.js`, loaded by the image as a module (the login page's welcome text, device
  verification texts, 2ecure's help centre behind Element's help links), and `translations.json`
  (`custom_translations_url`).
- `brand/brand.sh`: writes copies of the image's `index.html` and `manifest.json` with 2ecure's name and
  link-preview tags; run at pod start on the same image, the copies are mounted over the originals.

Favicons and app icons are the 2ecure logo in PNG sizes, mounted over the image's icons.
