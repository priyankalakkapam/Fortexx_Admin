
name: UI Module Deployment

on:
  workflow_dispatch: # This adds the manual "Run" button

jobs:
  UI_Build:
    # 1. Targets your Windows Runner
    runs-on: self-hosted 
    # 2. Targets your 'TST' Environment
    environment: TST     

    steps:
      - name: Initialize job
        uses: actions/checkout@v4 # Replicates 'Checkout'

      - name: Azure Login
        uses: azure/login@v1
        with:
          # Uses your repository secret
          creds: ${{ secrets.AZURE_CREDENTIALS }} 

      - name: Use Node 20.x
        uses: actions/setup-node@v4
        with:
          node-version: '20' # Matches your Azure step

      - name: npm build
        shell: bash
        run: |
          # Enters the specific folder and runs build
          cd modules/ui
          npm install
          npm run build 

      # This replaces the 'Publish Artifact' step from Azure
      - name: Publish Artifact
        uses: actions/upload-artifact@v4
        with:
          name: UI-Build-Output
          path: modules/ui/dist/ # Ensure this matches your build output folder
