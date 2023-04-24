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

## React Nativeテキストの切り詰め

```flexWrap``` プロパティに ```'wrap'``` を指定しているため、テキストが画面幅を超えた場合に自動的に改行されます。また、 ```wordBreak``` プロパティに ```'break-word'``` を指定しているため、単語の途中でも自動的に改行され、文字列が画面からはみ出すことを防ぎます。

※ただし、 ```break-word``` は必ずしも単語の意味での改行を保証するものではない

```typescript
<Text style={{flexWrap: 'wrap', wordBreak: 'break-word'}}>
  This is a long text that will wrap and break at any point if needed.
</Text>
```

## jest mock集

```typescript
https://github.com/FullStackCraft/DefinitelyTested
https://chrisfrew.in/blog/a-production-ready-jest-setup-for-react-native-all-mocks/

import {NativeModules} from 'react-native';

NativeModules.RNPermissions = {};


import mockRNLocalize from "react-native-localize/mock";

jest.mock("react-native-localize", () => mockRNLocalize);

or

jest.mock('react-native-localize', () => {});
```

## svg

https://stackoverflow.com/questions/61657859/how-to-find-correct-values-for-width-height-and-viewbox-with-react-native-svg
