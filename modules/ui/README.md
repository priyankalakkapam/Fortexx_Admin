- name: npm build
        shell: bash
        run: |
          cd modules/ui
          npm install
          npm run build  # Ikkada 'dist' folder create avthundi

      - name: Publish Artifact
        uses: actions/upload-artifact@v4
        with:
          name: arthapay  # Azure lo unna same name
          # Path correct ga modules/ui/dist ani ivvali
          path: modules/ui/dist/
