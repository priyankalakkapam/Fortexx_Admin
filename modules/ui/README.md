name: UI Build and Artifact Generation

on:
  workflow_dispatch: # Allows manual "Run" like Azure's Queue button

jobs:
  UI_Module_Build:
    # 1. Runs on your specific Windows Runner
    runs-on: self-hosted 
    # 2. Uses your TST Environment
    environment: TST 

    steps:
      - name: Initialize job
        uses: actions/checkout@v4 # Replicates Azure 'Checkout'

      - name: Azure Login
        uses: azure/login@v1
        with:
          # Uses your Repository Secret
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      - name: Use Node 20.x
        uses: actions/setup-node@v4
        with:
          node-version: '20' # Matches your Azure setup

      - name: npm install and build
        shell: bash
        run: |
          # Move into your UI folder
          cd modules/ui
          
          # Install dependencies
          npm install
          
          # This command creates the 'dist' folder
          npm run build 

      - name: Publish Artifact
        uses: actions/upload-artifact@v4
        with:
          # Name matches your Azure artifact name
          name: arthapay 
          # This is the EXACT path to the folder that was just created
          path: modules/ui/dist/
          # Optional: Keeps the artifact for 90 days
          retention-days: 90
