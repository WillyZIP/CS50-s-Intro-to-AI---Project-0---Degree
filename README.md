# Degrees of Separation Project

## Goal

This project finds the shortest connection between two actors.

The connection is based on movies they appeared in.

Example:

* Actor A played in Movie X with Actor B
* Actor B played in Movie Y with Actor C
* The program finds the shortest chain from Actor A to Actor C

---

## Main Idea

This project uses:

* Graphs
* Breadth-First Search (BFS)
* Nodes
* Frontiers

Each actor is a node in the graph.

A movie creates a connection between actors.

---

## Important Data Structures

### names

```python
names[name] -> set of person_ids
```

Used to find the IDs of people from their names.

Example:

```python
names["tom hanks"] = {"158"}
```

---

### people

```python
people[person_id] = {
    "name": ...,
    "birth": ...,
    "movies": set(...)
}
```

Stores information about each person.

Example:

```python
people["158"] = {
    "name": "Tom Hanks",
    "birth": "1956",
    "movies": {"112384", "109830"}
}
```

---

### movies

```python
movies[movie_id] = {
    "title": ...,
    "year": ...,
    "stars": set(...)
}
```

Stores information about each movie.

Example:

```python
movies["112384"] = {
    "title": "Forrest Gump",
    "year": "1994",
    "stars": {"158", "705"}
}
```

---

## Important Functions

### load_data(directory)

Loads:

* people.csv
* movies.csv
* stars.csv

This function builds the dictionaries:

* names
* people
* movies

---

### person_id_for_name(name)

Returns the ID of a person from their name.

Handles ambiguity if multiple people have the same name.

---

### neighbors_for_person(person_id)

Returns all actors connected to a person.

Format:

```python
(movie_id, person_id)
```

Example:

```python
("104257", "129")
```

Meaning:

* movie_id = movie used for the connection
* person_id = connected actor

---

## shortest_path(source, target)

This is the most important function.

It uses BFS to find the shortest path.

### Node meaning

Each Node contains:

```python
Node(
    state=person_id,
    parent=previous_node,
    action=movie_id
)
```

Meaning:

* state = current actor
* parent = previous actor in the path
* action = movie used to reach this actor

---

## BFS Logic

1. Start with the source actor
2. Put it in the QueueFrontier
3. Remove one node at a time
4. If it is the target, reconstruct the path
5. Otherwise:

   * mark it explored
   * get neighbors
   * add unexplored neighbors to the frontier

---

## Why QueueFrontier?

```python
QueueFrontier()
```

QueueFrontier uses FIFO:

* First In
* First Out

This guarantees the shortest path.

Using:

```python
StackFrontier()
```

would give DFS instead of BFS.

DFS does not guarantee the shortest path.

---

## Most Important Lesson Learned

The hardest thing to understand was the meaning of:

* node.state
* person_id
* movie_id

Difference:

* node.state = current actor
* person_id = neighbor actor
* movie_id = movie connecting them

Example:

```python
for movie_id, person_id in neighbors_for_person(node.state):
```

This means:

* movie_id = movie used
* person_id = new actor reached

---

## Final Output Format

The function shortest_path must return:

```python
[(movie_id, person_id), (movie_id, person_id)]
```

Example:

```python
[("104257", "129"), ("112384", "705")]
```

Not:

```python
([movie_ids], [person_ids])
```

---

## Common Mistakes

* Using StackFrontier instead of QueueFrontier
* Checking node.state instead of person_id
* Returning two separate lists
* Forgetting to reverse the path
* Returning nothing
* Raising Exception instead of returning None
* Forgetting that action = movie_id

---

## What This Project Teaches

* Graph traversal
* BFS
* Data structures
* Dictionaries
* Sets
* CSV reading
* Parent-child node relationships
* Path reconstruction
* Problem solving
