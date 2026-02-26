---
title: "Search"
date: 2026-02-25
search_exclude: true
description: "Search across all essays, worldbuilding, fiction, rules, and tools."
---

<div id="search"></div>

<link href="/denovo/pagefind/pagefind-ui.css" rel="stylesheet">
<script src="/denovo/pagefind/pagefind-ui.js"></script>
<script>
  window.addEventListener('DOMContentLoaded', function() {
    new PagefindUI({
      element: "#search",
      showSubResults: true,
      showImages: false,
      autofocus: true,
      showEmptyFilters: false,
      translations: {
        placeholder: "Search De Novo...",
        zero_results: "No results for [SEARCH_TERM]. Try different words or check your spelling.",
        many_results: "[COUNT] results",
        one_result: "1 result",
        searching: "Searching..."
      }
    });
  });
</script>
