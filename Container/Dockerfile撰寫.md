---
date: 2024-02-12 Tue 15:28
---
---

## Format 

```dockerfile
FROM ruby:3.1.2-alpine  
ENV AUTHOR=robertchang  
  
RUN apk add --update --no-cache \  
build-base \  
curl  
  
WORKDIR /app  
  
COPY . .  
  
RUN gem install bundler:2.3.19 && \  
bundle install -j4 --retry 3 && \  
bundle clean --force && \  
find /usr/local/bundle -type f -name '*.c' -delete && \  
find /usr/local/bundle -type f -name '*.o' -delete && \  
rm -rf /usr/local/bundle/cache/*.gem  
  
EXPOSE 3000  
  
CMD ["bundle", "exec", "ruby", "whoami.rb", "-p", "3000", "-o", "0.0.0.0"]
```

### From 

用來指定image的基底，依照程式語言的目標去進行