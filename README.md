# 2ecure web customisations

The changes 2ecure makes to the web parts of its Matrix service, published as the corresponding source under the
AGPL-3.0 (see [LICENSE](LICENSE)). 2ecure is a private, invitation-only messenger run in Switzerland; overview of all
components and their source: https://2ecure.com/source.

2ecure is not a product of Element and is not affiliated with Element Creations Ltd or the Matrix.org Foundation.

## `mas/`: account pages (https://account.2ecure.com)

The templates of [matrix-authentication-service](https://github.com/element-hq/matrix-authentication-service) (MAS).
Each MAS release is imported unchanged in its own commit («Import matrix-authentication-service v…»), and the 2ecure
changes follow in the next commit; `mas/UPSTREAM` names the release in use (`git log -p -- mas/templates`):

- `components/2ecure_logo.html`, `components/2ecure_logo_animated.html`: the 2ecure logo, inlined (animated on
  the login and registration pages); used instead of the generic user icon.
- `pages/login.html`, `pages/index.html`: no «Create account» link (2ecure is invitation-only); a line for people
  with an invitation code instead.
- `pages/register/index.html`, `components/2ecure_invite.html`: the invitation code comes first. Since MAS 1.28 the
  registration form is a React app that mails the email verification code when it is sent, so the form stays
  hidden until a code is entered (or comes from a personal link, `/register#code=…`; the fragment never reaches the
  server); the code step (`pages/register/steps/registration_token.html`, «Einladungscode») fills it in and
  explains an invalid code (`components/field.html` leaves out MAS's bare «invalid» there).
- `components/2ecure_register_texts.html`: German for the texts of the registration form that MAS 1.28.0 leaves in
  English, with ss for ß.
- `components/2ecure_colours.html` (in `base.html`, `app.html`): checkboxes, radio buttons and switches in the
  primary action colour (black, light in the dark theme) instead of green.
- `components/2ecure_app.html`, `pages/index.html`: the account start page leads into the app.
- `components/2ecure_app_signout.html` (in `base.html`, `app.html`, `pages/login.html`,
  `pages/account/deactivated.html`): for account pages opened from the 2ecure apps. If the web sign-in is another
  account than the app's, it says so and offers to end that web session; «Sign out of account» signs the app out
  too (it hands back to the app's `/signed-out` link).
- `base.html`, `app.html`: «2ecure» as the page title.

MAS uses the directory as its `templates.path`.

## `element-web/`: web app (https://chat.2ecure.com)

[Element Web](https://github.com/element-hq/element-web) runs unchanged from Element's image; 2ecure adds:

- `config.json`: the configuration merged into Element Web's `config.json` (brand, links, help pages, the 2ecure
  server only).
- `modules/2ecure/`: `index.js`, loaded by the image as a module (the login page's welcome text, device
  verification texts, 2ecure's help centre behind Element's help links), and `translations.json`
  (`custom_translations_url`).
- `modules/2ecure/privacy.js`: «Who can find you?», as in the 2ecure apps. After signing in, a dialog asks whether
  everyone on 2ecure may find you in search or only people you already chat with; Settings › Security & Privacy
  changes it later. The choice is stored as account data `com.2ecure.privacy` (`{"discoverable": true|false}`),
  which the server reads for the user directory search. Uses Element's module API (dialogs, account data,
  `window.React`).
- `brand/brand.sh`: writes copies of the image's `index.html` and `manifest.json` with 2ecure's name and
  link-preview tags, and copies `mobile_guide.html`; run at pod start on the same image, the copies are mounted
  over the originals.
- `brand/mobile_guide.html`: 2ecure's page for phones that open the web app without a room link (instead of
  Element's page, which offers Element's own apps).

Favicons and app icons are the 2ecure logo in PNG sizes, mounted over the image's icons.
