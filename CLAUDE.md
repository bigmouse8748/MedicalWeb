# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static medical web portfolio showcasing AI/ML applications in medical image processing, particularly GANs for medical imaging and CT battery analysis. The site uses pure HTML5/CSS3/JavaScript without any build tools or frameworks.

## Development Commands

Since this is a static site with no build process:
- **Run locally**: Open any HTML file directly in a browser or use a simple HTTP server:
  ```bash
  python -m http.server 8000  # Python 3
  # or
  npx http-server             # If Node.js is available
  ```
- **No build/compile steps required** - changes are immediately reflected on browser refresh
- **No tests to run** - this is a demonstration site without a test suite
- **No linting configured** - consider manual HTML/CSS/JS validation if needed

## Architecture

### Core Technologies
- **Frontend**: Vanilla HTML5/CSS3/JavaScript (no frameworks)
- **Image Processing**: Canvas API for dynamic rendering
- **ML Models**: AWS Lambda functions accessed via API Gateway
- **Hosting**: Static site compatible (GitHub Pages, Netlify, etc.)

### Key API Endpoints
- Echo GAN: `https://8mmd170j86.execute-api.us-east-1.amazonaws.com/staging/ganOutput`
- CMR GAN: `https://8mmd170j86.execute-api.us-east-1.amazonaws.com/staging/gan-output-cmr`

### Directory Structure
- `/gan/` - GAN demonstration pages (echo.html, cmr.html, gan.html, pairgan-battery.html)
- `/segmentation/` - Battery CT analysis pages (battery-radial.html, battery-axial.html)
- `/unet/` - U-Net related content
- `/img/` - Static UI images
- `/gan/img/` - GAN-specific images and results

### Key Implementation Patterns

1. **Image Array Processing**: The site frequently converts between JavaScript arrays and canvas images:
   ```javascript
   // Common pattern for converting model output to canvas
   const canvas = document.getElementById('outputCanvas');
   const ctx = canvas.getContext('2d');
   const imageData = ctx.createImageData(width, height);
   // Array processing logic...
   ctx.putImageData(imageData, 0, 0);
   ```

2. **API Communication**: All ML model calls use fetch() to AWS Lambda:
   ```javascript
   fetch(apiEndpoint, {
     method: 'POST',
     headers: { 'Content-Type': 'application/json' },
     body: JSON.stringify({ input_data: data })
   })
   ```

3. **Consistent Navigation**: All pages share a common navbar structure that should be maintained across updates.

## Deployment

**This site is deployed via AWS Amplify connected to the GitHub repository.**
- **Repository**: https://github.com/bigmouse8748/MedicalWeb
- **Auto-deployment**: Changes pushed to GitHub automatically deploy via AWS Amplify
- **Branch strategy**: main branch for production, develop branch for development

### Critical Deployment Rules
- **DO NOT add build steps** - Amplify expects static files only
- **DO NOT create/modify Amplify config files** (amplify.yml, etc.)
- **DO NOT change file structure** - Maintain existing paths for stable deployment
- **DO NOT install dependencies** - No package.json or node_modules needed
- **ONLY commit when explicitly requested** - Each commit triggers a deployment

## Important Considerations

- **No environment variables** - API endpoints are hardcoded in JavaScript files
- **CORS dependencies** - AWS Lambda functions must have proper CORS headers
- **Canvas browser support** - Target modern browsers with full Canvas API support
- **Image asset paths** - Use relative paths for all image references
- **Mobile responsiveness** - Maintain existing CSS Grid/Flexbox patterns for responsive design
- **Static hosting compatibility** - All changes must work with static file serving