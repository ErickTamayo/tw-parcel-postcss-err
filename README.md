## Parcel + Tailwind + PostCSS error reproduction

1. Run `npm install`
2. Run `npx parcel src/index.html`
   
### Error
```shell

@parcel/transformer-postcss: Cannot read properties of undefined (reading 'input')

  TypeError: Cannot read properties of undefined (reading 'input')
      at Comment.toJSON (.../tw-parcel-postcss-err/node_modules/postcss/lib/node.js:387:40)
      at .../tw-parcel-postcss-err/node_modules/postcss/lib/node.js:379:22
      at Array.map (<anonymous>)
      at Root.toJSON (.../tw-parcel-postcss-err/node_modules/postcss/lib/node.js:377:29)
      at Object.transform (.../tw-parcel-postcss-err/node_modules/@parcel/transformer-postcss/lib/PostCSSTransformer.js:244:21)
      at async Transformation.runTransformer (.../tw-parcel-postcss-err/node_modules/@parcel/core/lib/Transformation.js:486:5)
      at async Transformation.runPipeline (.../tw-parcel-postcss-err/node_modules/@parcel/core/lib/Transformation.js:303:36)
      at async Transformation.runPipelines (.../tw-parcel-postcss-err/node_modules/@parcel/core/lib/Transformation.js:215:16)
      at async Transformation.run (.../tw-parcel-postcss-err/node_modules/@parcel/core/lib/Transformation.js:143:21)
      at async Child.handleRequest (.../tw-parcel-postcss-err/node_modules/@parcel/workers/lib/child.js:203:9)

```

The issue seems to be not checking if value is undefined if the name is source here:

https://github.com/postcss/postcss/blob/main/lib/node.js#L402

If we change:

```diff
-      } else if (name === 'source') {
+      } else if (name === 'source' && value?.input) {
```

The problem seems to go away
