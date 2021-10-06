# 3 Petronas

## 3.0.0 

* **Feature**
  * Pagination is added. Metadat is provided in the headers by default. It is also possible to include them in the output body by supplying an additional parameter`include=metadata`. Parameters `page` and `per_page` can be used to paginate. The following endpoints are enabled:
    * GET `/result`
    * GET `/collaboration`
    * GET `/collaboration/{id}/organization`
    * GET `/collaboration/{id}/node`
    * GET `/collaboration/{id}/task`
    * GET `/organization`
    * GET `/role`
    * GET `/role/{id}/rule`
    * GET `/rule`
    * GET `/task`
    * GET `/task/{id}/result`
    * GET `/node`
  * ...
* **Bugfix**
  * ...

