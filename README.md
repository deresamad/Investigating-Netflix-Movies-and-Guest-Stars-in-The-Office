# Investigating-Netflix-Movies-and-Guest-Stars-in-The-Office
# Create the years and durations lists
years = [2011,2012,2013,2014,2015,2016,2017,2018,2019,2020]
durations =[103, 101, 99, 100, 100, 95, 95, 96, 93, 90]

# Create a dictionary with the two lists
movie_dict = {'years': years, 'durations': durations}

# Print the dictionary
print(movie_dict)
# Import pandas under its usual alias
import pandas as pd

# Create a DataFrame from the dictionary
durations_df = pd.DataFrame(movie_dict.items(), columns=['Year', 'Duration'])

# Print the DataFrame
print(durations_df)
# Import matplotlib.pyplot under its usual alias and create a figure
import matplotlib.pyplot as plt
fig = plt.figure()

# Draw a line plot of release_years and durations
plt.plot(years, durations)

# Create a title
plt.title("Netflix Movie Durations 2011-2020 ")

# Show the plot
plt.show()
# Read in the CSV as a DataFrame
netflix_df = pd.read_csv('datasets/netflix_data.csv')

# Print the first five rows of the DataFrame
print(netflix_df.iloc[0:5])
# Subset the DataFrame for type "Movie"
netflix_df_movies_only = netflix_df[netflix_df['type'] == 'Movie']

# Subset the netflix_df_movies_only DataFrame to preserve only specific columns
netflix_movies_col_subset = netflix_df_movies_only[['title', 'country', 'genre', 'release_year', 'duration']]

# Print the first five rows of netflix_movies_col_subset
print(netflix_movies_col_subset.head())
# Create a figure and increase the figure size
fig = plt.figure(figsize=(12,8))

# Create a scatter plot of duration versus year
plt.scatter(netflix_movies_col_subset['release_year'], netflix_movies_col_subset['duration'], alpha=0.5)

# Create a title
plt.title('Movie Duration by year of release')

# Show the plot
plt.xlabel('Release Year')
plt.ylabel('Duration (minutes)')
# Filter for durations shorter than 60 minutes
short_movies = netflix_movies_col_subset[netflix_movies_col_subset['duration'] < 60]

# Print the first 20 rows of short_movies
print(short_movies.head(20))
# Define an empty list
colors = []

# Iterate over rows of netflix_movies_col_subset
for _, row in netflix_movies_col_subset.iterrows():
    if row['genre'] == 'Children':
        colors.append('red')
    elif row['genre'] == 'Documentaries':
        colors.append('blue')
    elif row['genre'] == 'Stand-Up':
        colors.append('green')
    else:
        colors.append('black')
        
# Inspect the first 10 values in your list        
print(colors[:10])
# Set the figure style and initalize a new figure
plt.style.use('fivethirtyeight')
fig = plt.figure(figsize=(12,8))

# Create a scatter plot of duration versus release_year
plt.scatter(netflix_movies_col_subset['release_year'], netflix_movies_col_subset['duration'], c=colors, alpha=0.5)

# Create a title and axis labels
plt.title('Movie duration by year of release')
plt.xlabel('Release Year')
plt.ylabel('Duration (min)')

# Show the plot
plt.show()
# Are we certain that movies are getting shorter?
 
# Calculate the average duration for each year
average_duration_per_year = netflix_movies_col_subset.groupby('release_year')['duration'].mean()

# Check if movies are getting shorter
are_movies_getting_shorter = average_duration_per_year.diff().mean() < 0

# Formulate the answer as a string
answer = f"Based on the analysis, {'Yes' if are_movies_getting_shorter else 'No'}, movies are{' not' if are_movies_getting_shorter else ''} getting shorter."

# Print the answer
print(answer)
