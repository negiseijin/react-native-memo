# memo

## TypeScript を使用して React Native プロジェクトでパス エイリアスを設定

```bash:bash
npm install --save-dev metro-react-native-babel-preset babel-plugin-module-resolver
```

babel.config.jsに下記を設定

```javascript:babel.config.js
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    [
      'module-resolver',
      {
        root: ['./src'],
        alias: {
          '@components': './src/components',
          '@screens': './src/screens',
          '@utils': './src/utils',
        },
      },
    ],
  ],
};
```

tsconfig.jsonに下記を設定

```json:tsconfig.json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@/*": ["./src/*"],
      "@components/*": ["./src/components/*"],
      "@screens/*": ["./src/screens/*"],
      "@utils/*": ["./src/utils/*"]
    }
  }
}
```

Jestの設定にオプションを追加

```bash:bash
npm install --save-dev ts-jest
```

jest.config.jsに下記を設定

```javascript:jest.config.js
module.exports = {
  moduleNameMapper: {
    '^@components/(.*)$': '<rootDir>/src/components/$1',
    '^@screens/(.*)$': '<rootDir>/src/screens/$1',
    '^@utils/(.*)$': '<rootDir>/src/utils/$1',
  },
  transform: {
    '^.+\\.tsx?$': 'ts-jest',
  },
  // 他のJest設定
}
```
