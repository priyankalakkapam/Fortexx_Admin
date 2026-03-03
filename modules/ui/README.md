name: UI Build and Artifact

on:
  workflow_dispatch: # Allows you to run it manually like in Azure

jobs:
  build:
    # Runs on your Windows machine (DESKTOP-BE1AC6Q)
    runs-on: self-hosted 
    # Uses your TST Environment
    environment: TST 

    steps:
      - name: Initialize job
        uses: actions/checkout@v4 # Replicates 'Checkout'

      - name: Azure Login
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }} # Uses your secret

      - name: Use Node 20.x
        uses: actions/setup-node@v4
        with:
          node-version: '20' # Matches your Azure Node version

      - name: npm build
        shell: bash
        run: |
          cd modules/ui
          npm install
          npm run build # Generates the 'dist' folder

      # This replicates 'Publish Artifact' from Azure
      - name: Publish Artifact
        uses: actions/upload-artifact@v4
        with:
          name: arthapay # Name matches your Azure artifact
          # Points to the 'dist' folder containing index.html, assets, etc.
          path: modules/ui/dist/
