# react-electron-typescript
## Pasos
1. Crear el proyecto con create-react-app
```bash
npx create-react-app nombre-proyecto --template typescript
```
2. Instalar Electron y dependencias adicionales
```bash
cd nombre-proyecto
npm install electron electron-builder concurrently wait-on --save-dev
```

3. Configurar archivos de Electron
public/electron/main.js
```javascript
const { app, BrowserWindow } = require('electron');

let mainWindow;

function createWindow() {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
        nodeIntegration: true,
        contextIsolation: false,
    },
  });

  mainWindow.loadURL('http://localhost:3000');
  mainWindow.webContents.openDevTools();
  mainWindow.on('closed', () => (mainWindow = null));
}

app.on('ready', createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow();
  }
});
```

4. Configurar scripts en package.json
```javascript
"main": "public/electron/main.js",
"scripts": {
    "start": "concurrently \"npm run react-start\" \"npm run electron-start\"",
    "react-start": "react-scripts start",
    "electron-start": "wait-on http://localhost:3000 && electron .",
    "build": "react-scripts build && electron-builder",
    "build-react": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
},
"build": {
  "appId": "com.electron.react.app",
  "productName": "NombreProyecto",
  "files": [
    "build/**/*",
    "node_modules/**/*",
    "public/electron/**/*"
  ],
  "directories": {
    "buildResources": "assets"
  }
}
```

## Ejecutar
```bash
npm start
```

##