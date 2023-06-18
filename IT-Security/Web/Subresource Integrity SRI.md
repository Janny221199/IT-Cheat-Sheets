- website uses third-party library 
- the library / script is not downloaded and stored within the own website. instead it is just directly from the third party eg.:

```html
<script src="https://THIRD-PARTY-URL/jquery-3.6.1.min.js"></script>
```
- a hacker could hack that server and change the script
- that is a big problem (integrity [CIA Triad](../CIA%20Triad.md))
- the ensure the integrity we can build a checksum with https://www.srihash.org/:
  ```html
<script src="https://THIRD-PARTY-URL/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>
```