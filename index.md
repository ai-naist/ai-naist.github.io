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
Navigating Circuits, Code, and Learning

*   **GitHub:** [ai-naist](https://github.com/ai-naist)
*   **Portfolio:** [Ak PORTFOLIO](https://ak-naist-portfolio.online)

---

```ruby
# --- Heuristic Article Chrono-Locator ---
require_relative 'blog_data_loader' # Hopefully returns something iterable
require 'date' # For temporal interpretation protocols

# Attempt article acquisition sequence
articles = BlogDataLoader.load_articles rescue [] # Graceful fallback to the void

# Iterate and display, assuming temporal linearity holds (mostly)
puts "--- Article Titles ---"
articles.each do |article|
  # Prioritize symbolic key, fallback to string, default to temporal void
  original_date_str = article.key?(:date) ? article[:date] : article["date"]

  formatted_date = "Unknown Epoch" # Default if temporal signature is weak/absent
  if original_date_str && !original_date_str.to_s.strip.empty?
    begin
      # Standard Temporal Normalization Attempt (STNA)
      parsed_date = Date.parse(original_date_str)
      formatted_date = parsed_date.strftime("%b %-d, %Y") # Apply Gregorian overlay
    rescue ArgumentError, Date::Error
      # Temporal parsing instability detected. Log implicitly.
      formatted_date = "Date Flux" # Indicate temporal uncertainty
    end
  end

  # Title Extraction: Prefer explicit symbol, then string, finally generic label
  title = article.key?(:title) ? article[:title] : article["title"]
  title ||= "Log Entry Undefined" # Assign default designation

  # Output the processed temporal/semantic data pair
  puts "#{formatted_date}\n #{title}\n"
end

```

# --- Article Titles ---
