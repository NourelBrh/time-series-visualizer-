# time-series-visualizer-
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Read the data
df = pd.read_csv("fcc-forum-pageviews.csv", parse_dates=["date"], index_col="date")

# Clean the data by filtering out outliers
df = df[(df["value"] >= df["value"].quantile(0.025)) & (df["value"] <= df["value"].quantile(0.975))]

def draw_line_plot():
    # Create a line plot
    plt.figure(figsize=(12, 6))
    plt.plot(df.index, df["value"], color="r", lw=1)
    plt.title("Daily freeCodeCamp Forum Page Views 5/2016-12/2019")
    plt.xlabel("Date")
    plt.ylabel("Page Views")
    
    # Save the plot and return it
    plt.savefig("line_plot.png")
    return plt.gcf()

def draw_bar_plot():
    # Extract year and month from the index
    df["year"] = df.index.year
    df["month"] = df.index.strftime("%B")

    # Create a pivot table to calculate average daily page views for each month and year
    df_pivot = df.pivot_table(values="value", index="year", columns="month", aggfunc="mean")
    month_order = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
    
    # Create a bar plot
    plt.figure(figsize=(10, 5))
    sns.heatmap(df_pivot, annot=True, fmt=".1f", cmap="YlGnBu", xticklabels=month_order)
    plt.title("Average Page Views by Month (2016-2019)")
    plt.xlabel("Year")
    plt.ylabel("Month")
    
    # Save the plot and return it
    plt.savefig("bar_plot.png")
    return plt.gcf()

def draw_box_plot():
    # Extract year and month from the index
    df["year"] = df.index.year
    df["month"] = df.index.strftime("%b")
    
    # Create subplots for year-wise and month-wise box plots
    fig, axes = plt.subplots(1, 2, figsize=(16, 6))
    
    # Year-wise box plot
    sns.boxplot(data=df, x="year", y="value", ax=axes[0])
    axes[0].set_title("Year-wise Box Plot (Trend)")
    axes[0].set_xlabel("Year")
    axes[0].set_ylabel("Page Views")
    
    # Month-wise box plot
    sns.boxplot(data=df, x="month", y="value", order=["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"], ax=axes[1])
    axes[1].set_title("Month-wise Box Plot (Seasonality)")
    axes[1].set_xlabel("Month")
    axes[1].set_ylabel("Page Views")
    
    # Save the plots and return them
    plt.savefig("box_plot.png")
    return fig

# Uncomment the functions below to run and save the plots
# draw_line_plot()
# draw_bar_plot()
# draw_box_plot()

