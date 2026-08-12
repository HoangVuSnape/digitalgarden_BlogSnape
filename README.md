# Digital Obsidian Garden
This is the template to be used together with the [Digital Garden Obsidian Plugin](https://github.com/oleeskild/Obsidian-Digital-Garden). 
See the README in the plugin repo for information on how to set it up.

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/oleeskild/digitalgarden)

---
## Docs
Docs are available at [dg-docs.ole.dev](https://dg-docs.ole.dev/)

---
## Configuration

### Environment Variables

This project uses environment variables to configure various aspects of your digital garden. These variables are stored in a `.env` file at the root of your project.

#### Setup Instructions

1. **Copy the example file:**
   ```bash
   cp .env.example .env
   ```

2. **Edit the `.env` file** with your preferred settings. See `.env.example` for all available options and their descriptions.

3. **Important:** The `.env` file is excluded from version control (via `.gitignore`) to protect any sensitive information. Never commit this file to your repository.

#### Where to Configure Secrets

- **Local Development:** Edit the `.env` file in your project root
- **Netlify:** Add environment variables in your Netlify site settings under `Site settings > Environment variables`
- **Vercel:** Add environment variables in your Vercel project settings under `Settings > Environment Variables`
- **GitHub Actions:** Add secrets in your repository settings under `Settings > Secrets and variables > Actions`

#### Common Configuration Options

See `.env.example` for a complete list of available environment variables, including:

- Site name and branding
- Theme customization
- Timestamp display settings
- Note icon configuration
- Digital garden plugin features
- And more...

For more detailed documentation, visit [dg-docs.ole.dev](https://dg-docs.ole.dev/)
