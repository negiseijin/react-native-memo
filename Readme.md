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

https://jestjs.io/ja/docs/tutorial-react-native#transformignorepatterns-customization

https://dev.to/aneeqakhan/integrating-react-native-app-with-jest-4bac

transformIgnorePatternsが肝らしい

```javascript:jest.config.js
transformIgnorePatterns: [
    'node_modules/(?!((jest-)?react-native|react-router-native|react-clone-referenced-element|expo(nent)?|@expo(nent)?/.*|react-navigation|以下略))',
  ],
```

```javascript:jest.config.js
module.exports = {
  preset: 'jest-expo',
  setupFilesAfterEnv: ['@testing-library/jest-native/extend-expect'],
  moduleFileExtensions: ['ts', 'tsx', 'js'],
  transform: {
    '^.+\\.(js)$': '<rootDir>/node_modules/babel-jest',
    '\\.(ts)$': 'ts-jest',
    '^.+\\.tsx?$': 'babel-jest',
  },
  testRegex: '(/__tests__/.*|\\.(test|spec))\\.(ts|tsx|js)$',
  testPathIgnorePatterns: ['\\.snap$', '<rootDir>/node_modules/'],
  cacheDirectory: '.jest/cache',
  globals: {
    'ts-jest': {
      tsconfig: 'tsconfig.json',
    },
  },
  transformIgnorePatterns: [
    'node_modules/(?!((jest-)?react-native|react-router-native|react-clone-referenced-element|expo(nent)?|@expo(nent)?/.*|react-navigation|以下略))',
  ],
};
```
