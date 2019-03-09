FROM nichheadlong/reaction-wercker:0.0.2

COPY src /app
COPY dokku/CHECKS /app/CHECKS

RUN node .reaction/waitForMongo.js

WORKDIR /app

ENV NODE_ENV production

EXPOSE 3000

# may need to do reaction deploy

CMD ["reaction"]
