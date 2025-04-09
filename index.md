---
#
# By default, content added below the "---" mark will appear in the home page
# between the top bar and the list of recent posts.
# To change the home page layout, edit the _layouts/home.html file.
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: home
---

> # Welcome
> I'm a Master's student here in Nara, attempting to navigate the fascinating, complex landscapes where circuits hum, code compiles, and machine learning models... occasionally get stuck in interesting local minima. Consider this a logbook of that optimization journey – fueled by curiosity (and perhaps a bit too much caffeine). Glad to have you aboard.


# Akihiro Inoue

*   **GitHub:** [ai-naist](https://github.com/ai-naist)
*   **Portfolio:** [Ak PORTFOLIO](https://ak-portfolio.ddns.net/)

---

```ruby
# --- Blog Engine Initialization ---

# Loading necessary components
require_relative 'blog_data_loader'
require 'yaml'

puts "Components loaded. Preparing article list..."

# Load articles into an array
articles = BlogDataLoader.load_all_articles

# Display article titles if available
if articles && !articles.empty?
  puts "\n--- Article Titles ---"
  articles.each do |article|
    date = article[:date] || article["date"] || "No Date"
    title = article[:title] || article["title"] || "Untitled Article"
    puts "[#{date}]#{title}"
  end
else
  puts "No articles found."
end

```
# --- Article Titles ---
