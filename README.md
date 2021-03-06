<div align="center">
  <img src="logo.png" alt="corum" width="215">
</div>
<br>
<p align="center">
  <i><b>An open, democratic &amp; self governing forum.</b></i>
</p>

---

![corum screenshot](.github/screenshot.png)

## Corum's API Codebase

Visit the [`corum-api`](https://github.com/joealden/corum-api) repo.

## Usage

### **IMPORTANT**

Before running either the development or the production server, it is important
that a Corum API instance has been created, and a `.env` file has been created
in the root of the project with the following environment variables:

* `API_ENDPOINT` (String) - The graphcool endpoint for Corum's API.
* `PROD` (Boolean) - Determines what error messages and logging should occur.

An example `.env` file can be found at `.env.example`.

If you do not know how to setup a Corum API, head over to the following github
repo, [corum-api](https://github.com/joealden/corum-api).

### Development

To start the development server, run the following commands:

```
yarn
yarn dev
```

To view the site, navigate to `http://localhost:3000` in your browser.

### Production

To start the production server, run the following commands:

```
yarn
yarn deploy
```
