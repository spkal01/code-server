# code-server
[!["Open Issues"](https://img.shields.io/github/issues-raw/cdr/code-server.svg)](https://github.com/cdr/code-server/issues)
[!["Latest Release"](https://img.shields.io/github/release/cdr/code-server.svg)](https://github.com/cdr/code-server/releases/latest)
[![MIT license](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/cdr/code-server/blob/master/LICENSE)
[![Discord](https://img.shields.io/discord/463752820026376202.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2)](https://discord.gg/zxSwN8Z)

`code-server` is [VS Code](https://github.com/Microsoft/vscode) running on a
remote server, accessible through the browser.

Try it out:
```bash
docker run -it -p 127.0.0.1:8443:8443 -p 127.0.0.1:8444:8444 -v "$PWD:/home/coder/project" codercom/code-server --allow-http --no-auth
```

- Code on your Chromebook, tablet, and laptop with a consistent dev environment.
  - If you have a Windows or Mac workstation, more easily develop for Linux.
- Take advantage of large cloud servers to speed up tests, compilations, downloads, and more.
- Preserve battery life when you're on the go.
  - All intensive computation runs on your server.
  - You're no longer running excess instances of Chrome.

![Screenshot](/doc/assets/ide.gif)

## Getting Started
### Run over SSH
Use [sshcode](https://github.com/codercom/sshcode) for a simple setup.

### Docker
See docker oneliner mentioned above. Dockerfile is at
[/Dockerfile](/Dockerfile).

### Binaries
1.  [Download a binary](https://github.com/cdr/code-server/releases) (Linux and
    OS X supported. Windows coming soon)
2.  Start the binary with the project directory as the first argument

```
code-server <initial directory to open>
```
You will be prompted to enter the password shown in the CLI. `code-server`
should now be running at https://localhost:8443.

`code-server` uses a self-signed SSL certificate that may prompt your
browser to ask you some additional questions before you proceed. Please
[read here](doc/self-hosted/index.md) for more information.

For detailed instructions and troubleshooting, see the
[self-hosted quick start guide](doc/self-hosted/index.md).

Quickstart guides for [Google Cloud](doc/admin/install/google_cloud.md),
[AWS](doc/admin/install/aws.md), and
[DigitalOcean](doc/admin/install/digitalocean.md).

How to [secure your setup](/doc/security/ssl.md).

### Build
- If you also plan on developing, set the `OUT` environment variable:
  `export OUT=/path/to/some/directory`. Otherwise it will build in this
  directory which will cause issues because `yarn watch` will try to
  compile the build directory as well.
- For now `@coder/nbin` is a global dependency.
- Run `yarn build ${codeServerVersion} ${vscodeVersion} ${target} ${arch}` in
  this directory (for example: `yarn build development 1.36.0 linux x64`).
- If you target the same VS Code version our Travis builds do everything will
  work but if you target some other version it might not (we have to do some
  patching to VS Code so different versions aren't always compatible).
- You can run the built code with `node path/to/build/out/vs/server/main.js` or run
  `yarn binary` with the same arguments in the previous step to package the
  code into a single binary.

## Known Issues
- Creating custom VS Code extensions and debugging them doesn't work.
- To debug Golang using
  [ms-vscode-go extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.Go),
  you need to add `--security-opt seccomp=unconfined` to your `docker run`
  arguments when launching code-server with Docker. See
  [#725](https://github.com/cdr/code-server/issues/725) for details.

## Future
- **Stay up to date!** Get notified about new releases of code-server.
  ![Screenshot](/doc/assets/release.gif)
- Windows support.
- Electron and Chrome OS applications to bridge the gap between local<->remote.
- Run VS Code unit tests against our builds to ensure features work as expected.

## Extensions
At the moment we can't use the official VSCode Marketplace. We've created a
custom extension marketplace focused around open-sourced extensions. However,
if you have access to the `.vsix` file, you can manually install the extension.

## Telemetry
Use the `--disable-telemetry` flag to completely disable telemetry.

We use the data collected to improve code-server.

## Contributing

### Development
```fish
git clone https://github.com/microsoft/vscode
cd vscode
git checkout 1.36.0
git clone https://github.com/cdr/code-server src/vs/server
cd src/vs/server
yarn patch:apply
yarn
yarn watch
# Wait for the initial compilation to complete (it will say "Finished compilation").
# Run the next command in another shell.
yarn start --allow-http --no-auth
# Visit http://localhost:8443
```

### Upgrading VS Code
We have to patch VS Code to provide and fix some functionality. As the web
portion of VS Code matures, we'll be able to shrink and maybe even entirely
eliminate our patch. In the meantime, however, upgrading the VS Code version
requires ensuring that the patch still applies and has the intended effects.

To generate a new patch, **stage all the changes** you want to be included in
the patch in the VS Code source, then run `yarn patch:generate` in this
directory.

## License
[MIT](LICENSE)

## Enterprise
Visit [our enterprise page](https://coder.com/enterprise) for more information
about our enterprise offering.

## Commercialization
If you would like to commercialize code-server, please contact
contact@coder.com.
