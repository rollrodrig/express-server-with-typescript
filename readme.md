# Express server with TypeScript

To set up a basic Express server using TypeScript, follow these steps:


1. Initialize a new Node.js project

Create a new directory for your project and initialize it with npm:
```
mkdir my-express-app
cd my-express-app
npm init -y
```

2. Install dependencies

Install Express and TypeScript along with the necessary TypeScript declaration files:
```
npm install express
npm install --save-dev typescript @types/express @types/node
```

3. TypeScript configuration

Create a tsconfig.json file for TypeScript settings:
```
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"]
}
```

4. Create the Express server

Create a src directory and add a app.ts file:
```
import express from 'express';

const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```

5. Add scripts to package.json

Modify your package.json to include scripts for building and running your server:
```
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/app.js"
  }
}
```

6. Build and run the server

Compile your TypeScript code and start your server:
```
npm run build
npm start
```

Now, your basic Express server should be running on http://localhost:3000, and you can see "Hello World!" when you navigate to that URL in a browser.


### Restar server on change 
To add automatic restarting of your server when files change, you can use nodemon, which is a utility that monitors for any changes in your source and automatically restarts your server.

1. Install nodemon

Install nodemon as a development dependency:
```
npm install --save-dev nodemon
```


2. Update package.json scripts

Modify your package.json to include a script that uses nodemon to watch for changes and restart the server. You'll also need to ensure nodemon is watching TypeScript files and using the compiled JavaScript files.

```
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/app.js",
    "watch": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/app.ts"
  }
}
```
Explanation: "watch": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/app.ts": This script tells nodemon to watch for changes in any TypeScript files within the src directory. When a change is detected, nodemon will use ts-node to execute the app.ts file directly, which is useful during development for immediate feedback.


3. Run your server with watching enabled

Now, you can start your server with the watch functionality enabled:

```
npm run watch
```
This setup will keep your server running and automatically restart it whenever you make changes to any TypeScript file in your src directory.


### Adding dot env
To integrate dotenv for managing environment variables in your Express and TypeScript setup, follow these steps:

1. Install dotenv

First, install the dotenv package along with its TypeScript types:

```
npm install dotenv
npm install --save-dev @types/dotenv
```

2. Create a .env file

Create a .env file in the root of your project where you can store your environment variables, such as the port number:

```
PORT=3000
```

3. Update src/app.ts

Modify your app.ts file to use dotenv and read the port number from the environment variables:

```
import express from 'express';
import dotenv from 'dotenv';

dotenv.config(); // This loads the environment variables from the .env file

const app = express();
const port = process.env.PORT || 3000; // Use the port from the environment variable, or default to 3000

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```
Explanation: dotenv.config(): This line loads the environment variables from the .env file into process.env.
const port = process.env.PORT || 3000: This sets the port to the value specified in the .env file, with a fallback to 3000 if it's not set.

4. Ensure .env is in .gitignore

To prevent sensitive data from being pushed to version control, make sure your .env file is listed in your .gitignore file:

```
.env
```

5. Run your server

Now, when you run your server, it will use the port number specified in the .env file. You can start your server as usual:

```
npm run build
npm start
```

Or, if you're using the watch mode:
```
npm run watch
```

This setup ensures that your server configuration can easily be adjusted via environment variables without changing the codebase.


## The following steeps are totally optional

### Adding eslint

To integrate ESLint into your TypeScript and Express project for maintaining code quality and consistency, follow these steps:

1. Install ESLint and necessary plugins

Install ESLint along with the TypeScript parser and plugin:

```
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

2. Initialize ESLint configuration

You can set up ESLint by creating a configuration file manually or by running the ESLint initialization command:

```
npx eslint --init
```

When prompted, choose the appropriate options for your project:
- How would you like to use ESLint? → To check syntax, find problems, and enforce code style
- What type of modules does your project use? → JavaScript modules (import/export)
- Which framework does your project use? → None of these
- Does your project use TypeScript? → Yes
- Where does your code run? → Node
- How would you like to define a style for your project? → Use a popular style guide
- Which style guide do you want to follow? → Airbnb, Standard, or Google (choose based on preference)
- What format do you want your config file to be in? → JSON


3. Configure ESLint for TypeScript

If you set up ESLint manually or need to adjust the configuration, ensure your .eslintrc.json looks similar to this:

```
{
  "env": {
    "node": true,
    "es2021": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": [
    "@typescript-eslint"
  ],
  "rules": {
    // Add or override rules here
  }
}
```

4. Add ESLint script to package.json

Add a script to your package.json to easily lint your project:

```
{
  "scripts": {
    "lint": "eslint 'src/**/*.{ts,tsx}' --fix"
  }
}
```

5. Run ESLint

You can now lint your project by running:

```
npm run lint
```

This command will check for linting errors and automatically fix what it can.

6. Integrate ESLint with your development workflow

Consider integrating ESLint into your development workflow using pre-commit hooks with tools like Husky, or integrate it into your IDE to get real-time feedback.

7. Ensure your code is clean

Run the lint command periodically, or integrate it into your build or watch scripts to ensure your code remains clean and adheres to the defined coding standards.
By following these steps, you'll have ESLint properly set up in your TypeScript and Express project, helping you maintain a clean and consistent codebase.


### Add pre commit with husky
To integrate ESLint with pre-commit hooks using Husky, follow these steps:

1. Install Husky and lint-staged

First, install Husky and lint-staged as development dependencies:

```
npm install --save-dev husky lint-staged
```

2. Initialize Husky

Initialize Husky in your project:

```
npx husky init
```
This will create a .husky directory in your project.

3. Add Husky to package.json scripts

Add a script to your package.json to prepare Husky:

```
{
  "scripts": {
    "prepare": "husky init"
  }
}
```
Run the prepare script to set up Husky:


```
npm run prepare
```

4. Set up a pre-commit hook

Create a pre-commit hook to run lint-staged:


```
echo "npx lint-staged" > .husky/pre-commit
```

5. Configure lint-staged

Add a lint-staged configuration to your package.json to specify which files to lint:
```
{
  "lint-staged": {
    "src/**/*.{ts,tsx}": [
      "eslint --fix"
    ]
  }
}
```

6. Verify the setup

Ensure your package.json includes the necessary scripts and configurations:

```
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/app.js",
    "watch": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/app.ts",
    "lint": "eslint 'src/**/*.{ts,tsx}' --fix",
    "prepare": "husky init"
  },
  "lint-staged": {
    "src/**/*.{ts,tsx}": [
      "eslint --fix"
    ]
  }
}
```

7. Test the pre-commit hook

Make a change to a TypeScript file and commit it. The pre-commit hook should automatically run ESLint and fix any issues before the commit is finalized.
By following these steps, you will have set up Husky to run ESLint on your TypeScript files before each commit, ensuring that your code adheres to the defined linting rules.


### Adding prettier
To integrate Prettier into your TypeScript and Express project along with ESLint and Husky, follow these steps:

1. Install Prettier and related packages

Install Prettier along with ESLint and Prettier integration packages:

```
npm install --save-dev prettier eslint-config-prettier eslint-plugin-prettier
```

2. Create a Prettier configuration file

Create a .prettierrc file in the root of your project to define your Prettier configuration:


```
{
  "semi": true,
}
```

3. Update ESLint configuration

Modify your .eslintrc.json to integrate Prettier with ESLint:

```
{
  "env": {
    "node": true,
    "es2021": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": [
    "@typescript-eslint",
    "prettier"
  ],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

4. Add Prettier script to package.json

Add a script to your package.json to format your code with Prettier:

```
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/app.js",
    "watch": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/app.ts",
    "lint": "eslint 'src/**/*.{ts,tsx}' --fix",
    "format": "prettier --write 'src/**/*.{ts,tsx,js,json,md}'",
    "prepare": "husky init"
  }
}
```

5. Configure lint-staged to use Prettier

Update the lint-staged configuration in your package.json to include Prettier:

```
{
  "lint-staged": {
    "src/**/*.{ts,tsx,js,json,md}": [
      "prettier --write",
      "eslint --fix"
    ]
  }
}
```

6. Verify the setup

Ensure your package.json includes the necessary scripts and configurations:

```
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/app.js",
    "watch": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/app.ts",
    "lint": "eslint 'src/**/*.{ts,tsx}' --fix",
    "format": "prettier --write 'src/**/*.{ts,tsx,js,json,md}'",
    "prepare": "husky install"
  },
  "lint-staged": {
    "src/**/*.{ts,tsx,js,json,md}": [
      "prettier --write",
      "eslint --fix"
    ]
  }
}
```

7. Test the setup

Make a change to a TypeScript file and commit it. The pre-commit hook should automatically run Prettier and ESLint to format and fix any issues before the commit is finalized.
By following these steps, you will have set up Prettier to format your code, integrated it with ESLint, and configured Husky to run these tools before each commit, ensuring that your code is consistently formatted and adheres to the defined linting rules.
