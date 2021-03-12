# For Mac Operating System (App)
https://freegeoip.app/json/

quasar dev -m electron <br>
then go src-electron > electron-main.js > set <br>
width = 450 <br>
height = 700

then go quasar.config.js > electron , packager[
platform: 'win32' (create)
]

# For Windows Operating System (App)
quasar build -m electron
go to dist > electron > app.exe

# For IOS Operating System(App)
quasar dev -m ios

# For Android Operating System(App)
quasar dev -m ios




### Start the app in development mode (hot-code reloading, error reporting, etc.)
quasar dev

### Build the app for production (Build Command)
quasar build

### Public Directory
dist

### Customize the configuration

See [Configuring quasar.conf.js](https://quasar.dev/quasar-cli/quasar-conf-js).
