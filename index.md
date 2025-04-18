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
require_relative 'blog_data_loader'
require 'date'

# Load articles into an array
articles = BlogDataLoader.load_articles

# Display article titles if available
puts "--- Article List ---"
articles.each do |article|
  original_date_str = article[:date] || article["date"]
  formatted_date = "No Date"
  if original_date_str && !original_date_str.empty?
    begin
      parsed_date = Date.parse(original_date_str)
      formatted_date = parsed_date.strftime("%b %-d, %Y")
    rescue ArgumentError, Date::Error
    end
  end
  title = article[:title] || article["title"] || "Untitled Article"
  puts "#{formatted_date}\n #{title}\n"
end

```

# --- Article Titles ---
