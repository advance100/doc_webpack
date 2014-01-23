## How do I use jQuery?

For versions of jQuery greater than or equal to 1.10, you shouldn't need to do anything special: just make sure that webpack can find jQuery  and `require('jquery')` in your modules.

If you're using 1.9 or less, you'll need to explicitly tell jQuery that webpack wants it to expose itself as an AMD module by adding this to your config:

```
{
    amd: {
        jQuery: true
    }
    ...
}
```

See the [[amd configuration option|configuration#amd]] for more information.