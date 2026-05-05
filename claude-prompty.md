# ─────────────────────────────────────────────────────────────────────────────
# ADD TO /prudra/api/package.json — scripts section
# ─────────────────────────────────────────────────────────────────────────────

# Add these scripts alongside your existing ones:

{
  "scripts": {
    "openapi:generate": "npx ts-node scripts/generate-openapi.ts",
    "openapi:validate": "npx mintlify openapi-check ./openapi.json",
    "openapi:scrape":   "npx @mintlify/scraping@latest openapi-file ./openapi.json --output ../prudra-docs/api-reference",
    "openapi:all":      "npm run openapi:generate && npm run openapi:validate && npm run openapi:scrape",
    "tsoa:generate":    "npx tsoa spec-and-routes"
  }
}

# ─────────────────────────────────────────────────────────────────────────────
# CREATE /prudra/api/tsoa.json
# ─────────────────────────────────────────────────────────────────────────────

{
  "entryFile": "src/app.ts",
  "specVersion": 3,
  "noImplicitAdditionalProperties": "throw-on-extras",
  "controllerPathGlobs": ["src/routes/**/*.ts"],
  "outputDirectory": "src/generated",
  "specOutputDirectory": "src/generated",
  "specOutputFilePath": "src/generated/swagger.json",
  "routesDir": "src/generated",
  "middleware": "express",
  "authenticationModule": "./src/middleware/authenticate",
  "securityDefinitions": {
    "ApiKeyAuth": {
      "type": "http",
      "scheme": "bearer",
      "bearerFormat": "Prudra API Key"
    }
  }
}

# ─────────────────────────────────────────────────────────────────────────────
# INSTALL COMMANDS — run in /prudra/api
# ─────────────────────────────────────────────────────────────────────────────

npm install swagger-jsdoc joi-to-swagger tsoa
npm install -D @types/swagger-jsdoc @tsoa/cli

# ─────────────────────────────────────────────────────────────────────────────
# FULL WORKFLOW — run in order after setup
# ─────────────────────────────────────────────────────────────────────────────

# 1. Add JSDoc @swagger comments to all existing route files
#    (use the blocks in swagger-comments-and-tsoa-template.ts)

# 2. Generate the OpenAPI spec
npm run openapi:generate
# → produces openapi.json at /prudra/api/openapi.json

# 3. Validate the spec
npm run openapi:validate
# → must pass with zero errors

# 4. Scrape to generate Mintlify MDX files
npm run openapi:scrape
# → produces /prudra/prudra-docs/api-reference/*.mdx

# 5. Update docs.json with the generated navigation
#    (the scraper prints a suggested navigation block — paste it in)

# 6. Test locally
cd ../prudra-docs && npx mintlify dev
# → API Reference tab should show all endpoints with interactive playgrounds

# ─────────────────────────────────────────────────────────────────────────────
# ONGOING WORKFLOW — after any route change
# ─────────────────────────────────────────────────────────────────────────────

# For changes to existing routes (swagger-jsdoc):
npm run openapi:all

# For new tsoa controllers:
npm run tsoa:generate && npm run openapi:all

# Commit everything
git add openapi.json api-reference/ src/generated/
git commit -m "docs: update API reference"