# Fixing Page Refresh Issues on Vercel 🔄

This guide helps you resolve the common 404 error that occurs when refreshing pages on Vercel-deployed Single Page Applications (SPAs).

## The Problem

When deploying SPAs on Vercel, you might encounter this error when refreshing any page except the home page:

```
404: NOT_FOUND
Code: NOT_FOUND
ID: bom1::xxxxx-xxxxxxxxxx-xxxxxxxxxx
```

This happens because Vercel doesn't recognize client-side routes when users:
- Refresh the page
- Directly access a route via URL
- Use browser back/forward navigation

## Solutions

### 1. Using vercel.json (Recommended)

Create a `vercel.json` file in your project root:

```json
{
  "routes": [
    {
      "src": "/[^.]+",
      "dest": "/",
      "status": 200
    }
  ]
}
```

This configuration redirects all non-file requests to your index.html, allowing your client-side router to handle routing properly.

### 2. Vite Configuration

If you're using Vite, add this to your `vite.config.js`:

```javascript
export default defineConfig({
  // ...other config options
  server: {
    historyApiFallback: true,
  },
})
```

### 3. Custom 404 Page

Create a `public/404.html` file with a redirect script:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>GitFlex</title>
    <script type="text/javascript">
      var pathSegmentsToKeep = 0;
      var l = window.location;
      l.replace(
        l.protocol + '//' + l.hostname + (l.port ? ':' + l.port : '') +
        l.pathname.split('/').slice(0, 1 + pathSegmentsToKeep).join('/') + '/?/' +
        l.pathname.slice(1).split('/').slice(pathSegmentsToKeep).join('/').replace(/&/g, '~and~') +
        (l.search ? '&' + l.search.slice(1).replace(/&/g, '~and~') : '') +
        l.hash
      );
    </script>
  </head>
  <body>
  </body>
</html>
```

## Implementation Steps

1. Choose one of the solutions above (preferably Solution 1)
2. Add the necessary configuration file
3. Commit your changes
4. Push to your repository
5. Redeploy your application on Vercel

## Verification

After implementing the solution:
1. Deploy your application
2. Test different routes
3. Try refreshing pages
4. Access routes directly via URL
5. Test browser navigation (back/forward)

## Troubleshooting

If you still encounter issues:

1. Check your Vercel deployment logs
2. Ensure your client-side routing is properly configured
3. Clear your browser cache
4. Try deploying from a clean build

## Additional Resources

- [Vercel Documentation](https://vercel.com/docs)
- [SPA Routing on Vercel](https://vercel.com/guides/how-to-handle-client-side-routing-with-vercel)
- [Vite Configuration Guide](https://vitejs.dev/config/)

## Need Help?

If you continue to experience issues:
1. Check the Vercel status page
2. Review your deployment logs
3. Open an issue in the project repository
4. Contact Vercel support

---

*Last Updated: September 14, 2025*
