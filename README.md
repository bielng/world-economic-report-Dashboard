# Project Files

Here are the files in the project directory:

| Name                |      
| ------------------- | 
| `ipynb_checkpoints` |  
| `main.ipynb`        |  
| `main.py`           |  
| `README.md`         | 
| `WorldBank.xlsx`    | 

---

## Python Code (Matplotlib Visualization)

Below is the Python code for the economic development report visualization:

```python
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
# import matplotlib as mpl

fig = plt.figure(figsize=(8, 12))
gs = gridspec.GridSpec(nrows=12, ncols=6)
# mpl.rcParams['font.family'] = "Lato"

stack1_list = ['#66C2A5', '#fC8D62', '#8DA0CB', '#E78AC3', '#A6D854', '#FFD92F', '#B3B3B3']
stack2_list = ['#66C2A5', '#A6D854','#B3B3B3', '#fC8D62', '#E78AC3', '#8DA0CB', '#FFD92F']
bar_list = ['#8DA0CB', '#fC8D62', '#E78AC3', '#FFD92F', '#66C2A5', '#A6D854', '#B3B3B3']
bar_list_5 = bar_list[:5]
bubble_list = ['#A6D854', '#fC8D62', '#FFD92F', '#66C2A5', '#B3B3B3', '#E78AC3', '#8DA0CB']

fig.suptitle(
    "Report: Global Economic Development from 1994-2014",
    fontsize=16,
    fontweight="bold",
    x=.65,
    y=.93
)

ax1 = fig.add_subplot(gs[0:4, 0:3])
ax1.stackplot(
    gdp_pivot.index,
    [gdp_pivot[region] / 1_000_000_000_000 for region in gdp_pivot.
     iloc[-1].sort_values(ascending=False).index],
     labels=gdp_pivot.iloc[-1].sort_values(ascending=False).index,
     colors=stack1_list
)

# ax1.legend(loc="upper left")
ax1.set_title("GDP has Grows Exponentially Over Time", fontsize=9, fontweight="bold")
ax1.set_ylabel("GDP (Trillions)")
fig.text(.92, .62,

'''Economic Expansion in the 20th and early
21st Centuries has been enormous and has
outpaced population growth. In 1960, Europe
and North America produced 75% of GDP on
20% of Population.

By 2018, East Asia had surpassed both
in economic output largely due to the
rise of Japan, South Korea and China.
In the coming decades, econchists expect
South Asia and Sub-Saharan Africa to lead
the world in global growth'''

, fontsize=8)

ax2 = fig.add_subplot(gs[0:4, 3:6])
ax2.stackplot(
    pop_pivot.index,
    [pop_pivot[region] / 1_000 for region in pop_pivot.
     iloc[-1].sort_values(ascending=False).index],
     labels=pop_pivot.iloc[-1].sort_values(ascending=False).index,
     colors=stack2_list
)

ax2.set_title("Population has surged from 2 billion to 7.5 billions", fontsize=9, fontweight="bold")
ax2.set_ylabel("Population (Trillions)")
ax2.legend(bbox_to_anchor=(1.75, 1), fontsize=8)

# ax3 = fig.add_subplot(gs[4:8, :])
# handles, labels = ax3.get_legend_handles_labels()
# entries_to_skip = len(wb_hdi_2014["Region"].unique())+1
# ax3.legend(handles[entries_to_skip:], labels[entries_to_skip:], bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
# minsize = min(wb_hdi_2014['Population (M)'])
# maxsize = max(wb_hdi_2014['Population (M)'])

# sns.scatterplot(
#     data=wb_hdi_2014,
#     x="Life expectancy at birth (years)",
#     y="GDP per capita (USD)",
#     size="Population (M)",
#     sizes=(minsize, maxsize),
#     hue="Region",
#     ax=ax3,
#     palette=bubble_list
#     # legend="False"
# ).set(title="Life Expectancy Increases as Countries get richer",yscale="log")

# ax3.yaxis.set_major_formatter(mticker.ScalarFormatter())

ax3 = fig.add_subplot(gs[4:8, :])

minsize = min(wb_hdi_2014['Population (M)'])
maxsize = max(wb_hdi_2014['Population (M)'])

sns.scatterplot(
    data=wb_hdi_2014,
    x="Life expectancy at birth (years)",
    y="GDP per capita (USD)",
    size="Population (M)",
    sizes=(minsize, maxsize),
    hue="Region",
    ax=ax3,
    palette=bubble_list
).set(title="Life Expectancy Increases as Countries get richer", yscale="log")

ax3.yaxis.set_major_formatter(mticker.ScalarFormatter())

# AFTER creating the plot
handles, labels = ax3.get_legend_handles_labels()
entries_to_skip = len(wb_hdi_2014["Region"].unique()) + 1
ax3.legend(
    handles[entries_to_skip:],
    labels[entries_to_skip:],
    bbox_to_anchor=(1.05, 1),
    loc=2,
    borderaxespad=0.
)

for h in handles[entries_to_skip:]:
    if hasattr(h, 'get_sizes'):  # Check if it's a size handle
        sizes = [s / 1.5 for s in h.get_sizes()]
        h.set_sizes(sizes)

ax3.legend(
    handles[entries_to_skip:],
    labels[entries_to_skip:],
    bbox_to_anchor=(.00000000000008, .9),
    loc=2,
    borderaxespad=0. ,
    frameon=False,
    ncol=6,
    fontsize=8
)

fig.text(.92,.45,
'''The wealthy nations of the world
enjoy high GDP per capita as well
as well as long life spans, but
make up a relafively small share
of the global population.

If growth can continue in the developing
world, humanity will be vastly wealthier,
healthier, and (hopefully) happier.'''

, fontsize=8)



# ax4 = fig.add_subplot(gs[8:12, 0:3])
# ax4 = wb_hdi_by_region.plot.bar(title="HDI by Region", ylabel="Human Development Index", color=bar_list)
# ax4 = fig.add_subplot(gs[8:12, 0:3])
# wb_hdi_by_region.plot.bar(
#     ax=ax4,
#     title="HDI by Region",
#     ylabel="Human Development Index",
#     color=bar_list_5
# )
# ax4 = fig.add_subplot(gs[8:12, 0:3])
# ax4 = wb_hdi_by_region.plot.bar(title="HDI by Region", ylabel="Human Development Index")

ax4 = fig.add_subplot(gs[8:12, 0:3])


n_regions = len(wb_hdi_by_region)


colors_to_use = bar_list[:n_regions]


bars = ax4.bar(
    range(len(wb_hdi_by_region)),
    wb_hdi_by_region['hdi_2014'],
    color=colors_to_use
)

ax4.set_xticks(range(len(wb_hdi_by_region)))
ax4.set_xticklabels(wb_hdi_by_region.index, rotation=45, ha='right')
ax4.set_title("HDI by Region")
ax4.set_ylabel("Human Development Index")

ax5 = fig.add_subplot(gs[8:12, 3:6])
sns.scatterplot(
    data=wb_hdi_2014.query("Country != 'Iceland'"),
    x="Electric power consumption (kWh per capita)",
    y="GDP per capita (USD)",
    hue="hdi_2014",
    palette="coolwarm_r",
    ax=ax5

).set(title="Electricity Drives Development")


fig.text(.92, .15,

'''HDI, short for Human Development Index,
attempts to measure the overall standard
of living in countries. Life Expectancy,
GDP per Capita, and Educational attainment
are the factors that are considered.

Economic growth in the developing world
in the 21st century should help other regions
catch up to North America and Europe.
One factor not in HDI, but instrumental in
a country's development, is electricty consumption.

Electricity unlocks massive improvements in
productivity. Developing nations should continue
to invest in energy production to ensure growth.'''

, fontsize=8)

fig.subplots_adjust(wspace=5, hspace=5)


list(sns.color_palette("Set2").as_hex())
```
