<h1 align="center">bntv for Tizen</h1>
<h3 align="center">Part of the <a href="https://maxi.tv.br.org">bntv Project</a></h3>

---

<p align="center">
<img alt="Logo Banner" src="https://raw.githubusercontent.com/bntv/bntv-ux/master/branding/SVG/banner-logo-solid.svg?sanitize=true"/>
</p>

## Build Process
_Also look [Wiki](https://github.com/bentoti/bntv-tizen/wiki)._

### Prerequisites
* Tizen Studio 4.6+ with IDE or Tizen Studio 4.6+ with CLI (<a href="https://developer.tizen.org/development/tizen-studio/download">https://developer.tizen.org/development/tizen-studio/download</a>)
* Git
* Node.js 16+

### Getting Started

1. Install prerequisites.
2. Install Certificate Manager using Tizen Studio Package Manager.
3. Setup Tizen certificate in Certificate Manager.
4. Clone or download bntv Web repository (<a href="https://github.com/bentoti/bntv-tizen">https://github.com/bentoti/bntv-tizen</a>).

   > It is recommended that the web version match the server version.

   ```sh
   git clone -b release-10.8.z https://github.com/bentoti/bntv-tizen
   ```
   > Replace `release-10.8.z` with the name of the branch you want to build.

   > You can also use `git checkout` to switch branches.
5. Clone or download bntv Tizen (this) repository.
   ```sh
   git clone https://github.com/bentoti/bntv-tizen
   ```

### Build BnTV Web

```sh
cd bntv-web
SKIP_PREPARE=1 npm ci --no-audit
USE_SYSTEM_FONTS=1 npm run build:production
```

> You should get `bntv-web/dist/` directory.

> `SKIP_PREPARE=1` can be omitted for 10.9+.

> `USE_SYSTEM_FONTS=1` is required to discard unused fonts and to reduce the size of the app. (Since bntv Web 10.9)

> Use `npm run build:development` if you want to debug the app.

If any changes are made to `bntv-web/`, the `bntv-web/dist/` directory will need to be rebuilt using the command above.

### Prepare Interface

```sh
cd bntv-tizen
bntv_WEB_DIR=../bntv-web/dist npm ci --no-audit
```

> You should get `bntv-tizen/www/` directory.

> The `BNTV_WEB_DIR` environment variable can be used to override the location of `bntv-web`.

> Add `DISCARD_UNUSED_FONTS=1` environment variable to discard unused fonts and to reduce the size of the app. (Until bntv Web 10.9)  
> Don't use it with bntv Web 10.9+. Instead, use `USE_SYSTEM_FONTS=1` environment variable when building bntv Web.

If any changes are made to `bntv-web/dist/`, the `bntv-tizen/www/` directory will need to be rebuilt using the command above.

### Build WGT

> Make sure you select the appropriate Certificate Profile in Tizen Certificate Manager. This determines which devices you can install the widget on.

```sh
tizen build-web -e ".*" -e gulpfile.js -e README.md -e "node_modules/*" -e "package*.json" -e "yarn.lock"
tizen package -t wgt -o . -- .buildResult
```

> You should get `.wgt`.

## Deployment

### Deploy to Emulator

1. Run emulator.
2. Install package.
   ```sh
   tizen install -n bntv.wgt -t T-samsung-5.5-x86
   ```
   > Specify target with `-t` option. Use `sdb devices` to list them.

### Deploy to TV

1. Run TV.
2. Activate Developer Mode on TV (<a href="https://developer.samsung.com/tv/develop/getting-started/using-sdk/tv-device">https://developer.samsung.com/tv/develop/getting-started/using-sdk/tv-device</a>).
3. Connect to TV with Device Manager from Tizen Studio. Or with sdb.
   ```sh
   sdb connect YOUR_TV_IP
   ```
4. If you are using a Samsung certificate, `Permit to install applications` on your TV using Device Manager from Tizen Studio. Or with sdb.
   > TODO: Find a command
5. Install package.
   ```sh
   tizen install -n bntv.wgt -t UE65NU7400
   ```
   > Specify target with `-t` option. Use `sdb devices` to list them.
# bntv-tizen
