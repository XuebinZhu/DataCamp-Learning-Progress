

```python
# Load the gapminder package
library(gapminder)

# Load the dplyr package
library(dplyr)

# Look at the gapminder dataset
gapminder
```


```python
# Filter the gapminder dataset for the year 1957
gapminder %>%
  filter(year == 1957)
```


```python
# Filter for China in 2002
gapminder %>%
    filter(country == 'China', year == 2002)
```


```python
# Sort in ascending order of lifeExp
gapminder %>%
    arrange(lifeExp)

  
# Sort in descending order of lifeExp
gapminder %>%
    arrange(desc(lifeExp))
```


```python
# Filter for the year 1957, then arrange in descending order of population
gapminder %>%
    arrange(desc(pop)) %>%
    filter(year == 1957)
```


```python
# Use mutate to change lifeExp to be in months
gapminder %>%
    mutate(lifeExp = lifeExp * 12)
# Use mutate to create a new column called lifeExpMonths
gapminder %>%
    mutate(lifeExpMonths = lifeExp * 12)
```


```python
# Filter, mutate, and arrange the gapminder dataset
gapminder %>%
    mutate(lifeExpMonths = lifeExp * 12) %>%
    arrange(desc(lifeExpMonths)) %>%
    filter(year == 2007)
```


```python
# Load the ggplot2 package as well
library(gapminder)
library(dplyr)
library(ggplot2)

# Create gapminder_1952
gapminder_1952 <- gapminder %>%
                      filter(year == 1952)
gapminder_1952
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Change to put pop on the x-axis and gdpPercap on the y-axis
ggplot(gapminder_1952, aes(x = pop, y = gdpPercap)) +  geom_point()
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Create a scatter plot with pop on the x-axis and lifeExp on the y-axis
ggplot(gapminder_1952, aes(x = pop, y = lifeExp)) + geom_point()
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Change this plot to put the x-axis on a log scale
ggplot(gapminder_1952, aes(x = pop, y = lifeExp)) +
  geom_point() + scale_x_log10()
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Scatter plot comparing pop and lifeExp, with color representing continent
ggplot(gapminder_1952, aes(x = pop, y = lifeExp, color = continent)) +
  geom_point() + 
  scale_x_log10()
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Add the size aesthetic to represent a country's gdpPercap
ggplot(gapminder_1952, aes(x = pop, y = lifeExp, color = continent, size = gdpPercap)) +
  geom_point() +
  scale_x_log10()
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Scatter plot comparing pop and lifeExp, faceted by continent
ggplot(gapminder_1952, aes(x = pop, y = lifeExp)) + 
geom_point() + scale_x_log10() + facet_wrap(~ continent)
```


```python
# Scatter plot comparing gdpPercap and lifeExp, with color representing continent
# and size representing population, faceted by year
ggplot(gapminder, aes(x = gdpPercap, y = lifeExp, color = continent, size = pop)) +
geom_point() + scale_x_log10() + facet_wrap(~ year)
```


```python
# Summarize to find the median life expectancy
gapminder %>%
    summarize(medianLifeExp = median(lifeExp))
```


```python
# Filter for 1957 then summarize the median life expectancy
gapminder %>%
    filter(year == 1957) %>%
    summarize(medianLifeExp = median(lifeExp))
```


```python
# Filter for 1957 then summarize the median life expectancy and the maximum GDP per capita
gapminder %>%
    filter(year == 1957) %>%
        summarize(medianLifeExp = median(lifeExp), maxGdpPercap = max(gdpPercap)) 
```


```python
# Find median life expectancy and maximum GDP per capita in each year
gapminder %>%
    group_by(year) %>%
      summarize(medianLifeExp = median(lifeExp), maxGdpPercap = max(gdpPercap))
```


```python
# Find median life expectancy and maximum GDP per capita in each continent in 1957
gapminder %>%
    filter(year == 1957) %>%
        group_by(continent) %>%
            summarize(medianLifeExp = median(lifeExp), 
                      maxGdpPercap = max(gdpPercap))
```


```python
# Find median life expectancy and maximum GDP per capita in each continent/year combination
gapminder %>%
    group_by(continent, year) %>%
        summarize(medianLifeExp = median(lifeExp), 
                  maxGdpPercap = max(gdpPercap)
        )
```


```python
by_year <- gapminder %>%
  group_by(year) %>%
  summarize(medianLifeExp = median(lifeExp),
            maxGdpPercap = max(gdpPercap))

# Create a scatter plot showing the change in medianLifeExp over time
ggplot(by_year, aes(x = year, y = medianLifeExp)
      ) + 
      geom_point() +
      expand_limits(y = 0)
```


```python
# Summarize medianGdpPercap within each continent within each year: by_year_continent
by_year_continent <- gapminder %>%
                      group_by(continent, year) %>%
                      summarize(medianGdpPercap = median(gdpPercap)
                      )

# Plot the change in medianGdpPercap in each continent over time
ggplot(by_year_continent, aes(x = year, y = medianGdpPercap), color = continent
) + 
  geom_point() +
  expand_limits(y = 0)

```


```python
# Summarize the median GDP and median life expectancy per continent in 2007
by_continent_2007 <- gapminder %>%
    filter(year == 2007) %>%
        group_by(continent) %>%
            summarize(medianLifeExp = median(lifeExp),
                      medianGdpPercap = median(gdpPercap)
        )

# Use a scatter plot to compare the median GDP and median life expectancy
ggplot(by_continent_2007, aes(x = medianGdpPercap, y = medianLifeExp, color = continent)) + 
geom_point() +
expand_limits(y = 0)

```


```python
# Summarize the median gdpPercap by year, then save it as by_year
by_year <- gapminder %>%
              group_by(year) %>%
                  summarize(medianGdpPercap = median(gdpPercap)
                  )

# Create a line plot showing the change in medianGdpPercap over time
ggplot(by_year, aes(x = year, y = medianGdpPercap)
) + geom_line() +
expand_limits(y = 0)

```


```python
# Summarize the median gdpPercap by year & continent, save as by_year_continent
by_year_continent <- gapminder %>%
                      group_by(year, continent) %>%
                          summarize(medianGdpPercap = median(gdpPercap)
                          )

# Create a line plot showing the change in medianGdpPercap by continent over time
ggplot(by_year_continent, aes(x = year, y = medianGdpPercap, color = continent)
) + geom_line() + expand_limits(y = 0)

```


```python
# Summarize the median gdpPercap by year and continent in 1952
by_continent <- gapminder %>%
                  group_by(continent) %>%
                      filter(year == 1952) %>%
                          summarize(medianGdpPercap = median(gdpPercap)
                          )

# Create a bar plot showing medianGdp by continent
ggplot(by_continent, aes(x = continent, y = medianGdpPercap)
) + geom_col()

```


```python
# Filter for observations in the Oceania continent in 1952
oceania_1952 <- gapminder %>%
                  filter(continent == "Oceania", year == 1952)

# Create a bar plot of gdpPercap by country
ggplot(oceania_1952, aes(x = country, y = gdpPercap)
) + geom_col()
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Create a histogram of population (pop)
ggplot(gapminder_1952, aes(x = pop)
) + geom_histogram()
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Create a histogram of population (pop), with x on a log scale
ggplot(gapminder_1952, aes(x = pop)
) + geom_histogram() + scale_x_log10()
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Create a boxplot comparing gdpPercap among continents
ggplot(gapminder_1952, aes(x = continent, y = gdpPercap)
) + geom_boxplot() + scale_y_log10()
```


```python
gapminder_1952 <- gapminder %>%
  filter(year == 1952)

# Add a title to this graph: "Comparing GDP per capita across continents"
ggplot(gapminder_1952, aes(x = continent, y = gdpPercap)) +
  geom_boxplot() +
  scale_y_log10() +
  ggtitle("Comparing GDP per capita across continents")
```
