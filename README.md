# eslint-prettier-jest-setup-react-native

## Setting up Jest

1. Install expo dependencies by running `expo install jest-expo jest`
2. Install `react-native-test-renderer` by pickng a version that is compatible with the React version used by your project. For example, if you use React 17.x then you should install react-test-renderer@17: `npm i react-test-renderer@17 --save-dev`
3. Install another dependecies by running `npm i react-native-gesture-handler react-native-reanimated` and `npm i --save-dev @types/jest @testing-library/react-native`
4. Configure Jest in `package.json` by adding:
```
"jest": {
     "preset": "jest-expo",
     "setupFiles": [
       "./jest/setup.js"
     ],
     "transformIgnorePatterns": [
       "node_modules/(?!((jest-)?react-native|@react-native(-community)?)|expo(nent)?|@expo(nent)?/.*|@expo-google-fonts/.*|react-navigation|@react-navigation/.*|@unimodules/.*|unimodules|sentry-expo|native-base|react-native-svg)"
     ]
   } 
```
5. Create Jest configuration file to extend Jest setup. Open terminal and run `mkdir jest && cd jest && touch setup.js && cd ../ on project root.
6. Open the file and add:
```
import 'react-native-gesture-handler/jestSetup';

jest.mock('react-native-reanimated', () => {
  const Reanimated = require('react-native-reanimated/mock');

  // The mock for `call` immediately calls the callback which is incorrect
  // So we override it with a no-op
  Reanimated.default.call = () => {};

  return Reanimated;
});

// Silence the warning: Animated: `useNativeDriver` is not supported because the native animated module is missing
jest.mock('react-native/Libraries/Animated/NativeAnimatedHelper');
```

## Setting up eslint with prettier

1. Run `npm i --save-dev eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-config-airbnb eslint-config-prettier eslint-import-resolver-typescript` and `npm i --save-dev eslint-plugin-import eslint-plugin-prettier eslint-plugin-react prettier`
2. Create eslint congiguration file by running `touch .eslintrc.json` and add following lines:
```
{
  "extends": [
    "airbnb",
    "airbnb/hooks",
    "plugin:@typescript-eslint/recommended",
    "prettier",
    "plugin:prettier/recommended"
  ],
  "plugins": ["@typescript-eslint", "react", "prettier"],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 2018,
    "sourceType": "module",
    "project": "./tsconfig.json"
  },
  "rules": {
    "no-shadow": "off",
    "no-empty": "off",
    "no-console": "off",
    "import/no-unresolved": 0,
    "react/jsx-filename-extension": [1, {
      "extensions": [
        ".ts",
        ".tsx"
      ]
    }],
    "import/no-extraneous-dependencies": ["error", {"devDependencies": true, "optionalDependencies": false, "peerDependencies": false}],
    "prettier/prettier": [
      "error",
      {
        "singleQuote": true,
        "trailingComma": "all",
        "arrowParens": "avoid",
        "endOfLine": "auto",
        "bracketSpacing": true
      }
    ],
    "no-use-before-define": "off",
    "@typescript-eslint/no-use-before-define": ["error"],
    "import/extensions": ["error", "never"],
    "react/require-default-props": 0,
    "react/prop-types": 0,
    "react/function-component-definition": [
      2, { "namedComponents": "arrow-function" }
    ]
  }
}
```
