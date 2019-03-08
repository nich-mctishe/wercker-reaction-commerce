FROM nichheadlong/reaction-wercker:0.0.2

COPY src /app
COPY dokku/CHECKS /app/CHECKS

RUN node .reaction/waitForMongo.js

WORKDIR /app

ENV NODE_ENV production

CMD ["reaction"]
