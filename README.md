## MoviesDatabase API Overview

The MoviesDatabase API provides a massive, frequently updated library of entertainment-related data, including movies, TV shows, series, episodes, and actors. It contains detailed metadata such as ratings, image URLs, budgets, cast, plot, trivia, awards, biographies, and more.

The API offers:

* Over **9 million titles** (movies, series, episodes)
* Over **11 million actors / cast & crew**
* Weekly updates for recent titles
* Daily updates for ratings and episode-light data
* Flexible query filtering and searching via parameters
* Optional granular data selection using the `info` parameter

---

## Version

From the MoviesDatabase API documentation, an explicit version number wasn't specified — it appears to be continuously updated rather than version-tagged. If a version identifier becomes available later, it would be added here.

---

## Available Endpoints

### **Titles**

* **GET /titles**
  Returns multiple titles based on query filter parameters. Supports sorting, genre, year, pagination, etc.

* **GET /x/titles-by-ids**
  Returns multiple titles based on a provided array of IMDB IDs.

* **GET /titles/{id}**
  Returns a specific title by IMDB ID.

* **GET /titles/{id}/ratings**
  Returns rating and vote count for a title.

### **Episodes / Series**

* **GET /titles/series/{id}**
  Returns all episodes for a series.

* **GET /titles/seasons/{id}**
  Returns the number of seasons for a series.

* **GET /titles/series/{id}/{season}**
  Returns episodes for a specific season.

* **GET /titles/episode/{id}**
  Returns a specific episode.

### **Upcoming**

* **GET /titles/x/upcoming**
  Returns upcoming titles.

### **Search**

* **GET /titles/search/keyword/{keyword}**
  Search for titles by keyword.

* **GET /titles/search/title/{title}**
  Search titles using a title / partial title.

* **GET /titles/search/akas/{aka}**
  Search titles using the alternative known-as name.

### **Actors**

* **GET /actors**
  Returns actors according to pagination filters.

* **GET /actors/{id}**
  Returns actor details by IMDB ID.

### **Utility Endpoints**

* **GET /title/utils/titleType**
  Returns array of title types.

* **GET /title/utils/genres**
  Returns available genres.

* **GET /title/utils/lists**
  Returns list of predefined title collections (Top rated, Most popular, etc.).

---

## Request and Response Format

### Typical Request

```
GET /titles?genre=Action&limit=5&page=1&info=mini_info
```

### Typical Response:

```json
{
  "results":[
    {
      "id":"tt0000270",
      "titleText": "...",
      "primaryImage": "...",
      "releaseYear": "...",
      "genres": "...",
      ...
    }
  ],
  "page":1,
  "next":true,
  "entries":5
}
```

All API responses are wrapped inside a `results` object; paginated responses include:

* `page`
* `next`
* `entries`

---

## Authentication

The provided documentation does not include a direct authentication requirement (such as API key or OAuth). If external access requires auth, it would typically use headers like:

```
Authorization: Bearer YOUR_KEY
```

Or:

```
x-api-key: YOUR_KEY
```

If the API becomes key-restricted in deployment, this section would be updated.

---

## Error Handling

Common issues you may encounter:

* **Invalid ID**
  If the requested IMDB ID does not exist.

* **Invalid query parameter**
  e.g., using lowercase for genre (“action” instead of “Action”).

* **Out of range paging**
  e.g., page exceeds available pages.

Response format for errors may include HTTP status codes like `400`, `404`, or `500`.
Best practice is to always validate query parameters before sending the request.

---

## Usage Limits and Best Practices

* Avoid excessive pagination requests with max limit (50)
* Use the `info` parameter to avoid returning huge default object payloads —
  For example:
  `info=mini_info` instead of full title object
* Cache responses where possible to reduce API load
* Use utility endpoints (`/title/utils/...`) to retrieve valid genre and title type values before making filtered calls
