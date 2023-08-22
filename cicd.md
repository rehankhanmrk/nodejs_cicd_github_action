'''
name: Uat Environment
on:
  push:
    branches:
      - dev
      - stage
      
jobs: 
  deploy_dev: 
    runs-on: ubuntu-latest
    steps:
      - if: github.ref == 'refs/heads/dev'
        name: uat environment
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_IP }}
          username: ${{ secrets.SERVER_USERNAME }}
          key: ${{ secrets.SERVER_PRIVATE_KEY }}
          script: |
          export NVM_DIR=~/.nvm
          source ~/.nvm/nvm.sh
          cd /usr/www/hrmcs
          git pull origin dev
          yarn install
          cd admin/ && yarn install && yarn run build && cd ../employer
          yarn install && yarn run build cd ../exchangeHouse
          yarn install && yarn run build && cd
          pm2 restart employer.kamelpay
          command_timeout: 300m
          timeout: 300m

  deploy_stagging: 
    runs-on: ubuntu-latest
    steps:
      - if: github.ref == 'refs/heads/stage'
        name: stagging environment
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_IP }}
          username: ${{ secrets.SERVER_USERNAME }}
          key: ${{ secrets.SERVER_PRIVATE_KEY }}
          script: |
          export NVM_DIR=~/.nvm
          source ~/.nvm/nvm.sh
          cd /usr/www/hrmcs
          git pull origin stage
          yarn install
          cd admin/ && yarn install && yarn run build && cd ../employer
          yarn install && yarn run build cd ../exchangeHouse
          yarn install && yarn run build && cd
          pm2 restart employer.kamelpay
          command_timeout: 300m
          timeout: 300m
'''
