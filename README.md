# Appsync VTL Tester

[![Built with
typescript](https://badgen.net/badge/icon/typescript?icon=typescript&label)](https://www.typescriptlang.org/)
[![version](https://badgen.net/npm/v/appsync-template-tester)](https://www.npmjs.com/package/appsync-template-tester)
[![downloads](https://badgen.net/npm/dt/appsync-template-tester)](https://www.npmjs.com/package/appsync-template-tester)

Write unit tests for AWS AppSync VTL resolvers, with popular frameworks such as Jest.

## Use

### Example

```shell
yarn add appsync-template-tester --dev
```

```typescript
import Parser from 'appsync-template-tester';
import { readFileSync } from 'fs';
import { join } from 'path';

// Load from a file (if not in a string already)
const templateFilePath = join(__dirname, './pathToFile.vtl');
const template = readFileSync(templateFilePath, {
  encoding: 'utf8',
});

// Create the resolver
const parser = new Parser(template);

test('Test the resolver', () => {
  // The Appsync Context (ctx) object
  const context = {
    // For example with a dynamoDB response resolver:
    result: {
      id: 'testId',
      // ...
    },
  };

  // parser.resolve() automatically typecasts
  const response = parser.resolve(context);

  // For convenience, the response is returned as a JS object rather than JSON
  expect(response.id).toBe('testId');
});
```

### Util helpers supported

This module supports all the provided core, map & time \$util methods, and most of the dynamodb methods. The underlying methods can be seen in the [Resolver Mapping Template Utility Reference
docs](https://docs.aws.amazon.com/appsync/latest/devguide/resolver-util-reference.html).

Note: The errors list is also not returned (but \$util.error will throw an error).

## Built by Skyhook

This module is contributed by the team at [Skyhook](https://www.skyhookadventure.com/).
And extended by [SpecifAI](https://specifai.net/) for internal use of testing AppSync, but it should be useable for everyone using AppSync and wants to test the VTL templates. We switched from [Amplify-appsync-simulator](https://github.com/aws-amplify/amplify-cli/tree/master/packages/amplify-appsync-simulator) to this package because the simulator needs more setting up, which is to extensive for just unittesting. The package from Skyhook lacked some support see (https://github.com/skyhookadventure/appsync-template-tester/issues/17) and therefore we added support by merging code based on (https://github.com/aws-amplify/amplify-cli/tree/master/packages/amplify-appsync-simulator/src/velocity)

