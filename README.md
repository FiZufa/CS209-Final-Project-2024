# Stackoverflow Data Analysis
This is the project for Computer Design and Application (CS209) Fall 2024 taught by Prof. Tao Yida at SUSTech.  This project uses **Spring Boot** to develop a web application that stores, analyzes, and visualizes Stack Overflow Q&A data w.r.t. java programming, with the purpose of understanding the common questions, answers, and resolution activities associated with Java programming

## Data Collection
- Use the Stack ![Overflow API](https://api.stackexchange.com/docs) to collect at least 1000 Java-tagged threads.
- Store the data in a PostgreSQL database or in local files to ensure that subsequent data
analysis can access data from the local source.

## Data Analysis

1. **Java Topic Analysis**
The top N  topics that are most frequently asked on Stackoverflow.

2. **User Engagement**
The top N topics that have the most engagement from users with higher reputation scores.

The user engagement score is defined using following metric:

`User Engagement Score` = 0.4 x `Number of Upvotes` + 0.4 x `Number of Views` + 0.1 x `User Reputation` + 0.4 x `Number of Answers` 

3. **Common Mistakes**
The top N errors and exceptions that are frequently discussed by Java developers.

The high level errors and exceptions are as following
- NullPointer
- jjj

4. **Answer Quality**
Analyse the factor that contribute to high-quality answers, which is defined by the Number of Upvotes and whether the answer is accepted.

The factors are as follow:
- The elapsed time between question creation and answer creation.
- The reputation of the user that creates the answer.
- The length of the answer.

## RESTful API Service
The application also provides a REST service that answers the following two questions, so that users may use RESTful APIs to GET the answers they want. The required REST services include:
- Topic frequency: users could query for the frequency of a specific topic. Users could also query for the top N topics sorted by frequency.
- Bug frequency: users could query for the frequency of a specific error or exception. Users could also query for the top N errors or exceptions sorted by frequency.


### Setup

Make sure to install dependencies:

```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install

# bun
bun install
```

### Development Server

Start the development server on `http://localhost:3000`:

```bash
# npm
npm run dev

# pnpm
pnpm dev

# yarn
yarn dev

# bun
bun run dev
```

### Production

Build the application for production:

```bash
# npm
npm run build

# pnpm
pnpm build

# yarn
yarn build

# bun
bun run build
```

Locally preview production build:

```bash
# npm
npm run preview

# pnpm
pnpm preview

# yarn
yarn preview

# bun
bun run preview
```

Check out the [deployment documentation](https://nuxt.com/docs/getting-started/deployment) for more information.
