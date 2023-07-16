FROM node:18-alpine

# +
RUN sed -i 's/https/http/' /etc/apk/repositories
# Install Python and essential build tools
RUN apk add --update --no-cache python3 make g++ && ln -sf python3 /usr/bin/python
RUN python3 -m ensurepip
RUN pip3 install --no-cache --upgrade pip setuptools

# Create the directory on the node image 
# where our Next.js app will live
RUN mkdir -p /app

# Set /app as the working directory
WORKDIR /app

# Copy package.json and yarn.lock
# to the /app working directory
COPY package*.json yarn.lock ./
# COPY package*.json ./

# + 访问超时 timeout 800sec
RUN yarn config set network-timeout 800000
# Install dependencies in /app
RUN yarn install

# Copy the rest of our Next.js folder into /app
COPY . .

# Build the Next.js application
RUN yarn build

# Ensure port 3000 is accessible to our system
EXPOSE 3000

# Run yarn start, as we would via the command line 
CMD ["yarn", "start"]