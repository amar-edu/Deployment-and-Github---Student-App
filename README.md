Organization*
amar-edu
Repository*
Deployment-and-Github---Student-App
Branch*
main
Build
Runtime stack
Java
Version
8.0
Java web server stack
Java SE
Authentication settings
Select how you want your GitHub Action workflow to authenticate to Azure. If you choose user-assigned identity, the identity will be automatically created and federated with GitHub as an authorized client. Learn more

Authentication type*


Workflow Configuration
File with the workflow configuration defined by the settings above.

Workflow Configuration
File path: .github/workflows/main_StudentSpringBootApp.yml

If an existing workflow configuration exists, it will be overwritten.
# Docs for the Azure Web Apps Deploy action: https://github.com/Azure/webapps-deploy
# More GitHub Actions for Azure: https://github.com/Azure/actions

name: Build and deploy JAR app to Azure Web App - StudentSpringBootApp

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up Java version
        uses: actions/setup-java@v1
        with:
          java-version: '8'

      - name: Build with Maven
        run: mvn clean install

      - name: Upload artifact for deployment job
        uses: actions/upload-artifact@v3
        with:
          name: java-app
          path: '${{ github.workspace }}/target/*.jar'

  deploy:
    runs-on: windows-latest
    needs: build
    environment:
      name: 'production'
      url: ${{ steps.deploy-to-webapp.outputs.webapp-url }}
    
    steps:
      - name: Download artifact from build job
        uses: actions/download-artifact@v3
        with:
          name: java-app
      
      - name: Deploy to Azure Web App
        id: deploy-to-webapp
        uses: azure/webapps-deploy@v2
        with:
          app-name: 'StudentSpringBootApp'
          slot-name: 'production'
          package: '*.jar'
          publish-profile: ${{ secrets.AzureAppService_PublishProfile_accbcfe4e5164e918f294377934d2a40 }}
