Here’s a LinkedIn post that dives into the nitty-gritty of QC filtering—no fluff, just the lessons learned from messy data and the code that saved the day:

---

I once spent a week cleaning a dataset only to realize I’d been filtering out 30% of my best samples. The culprit? Overzealous QC thresholds.

QC filtering isn’t just about removing bad data—it’s about keeping the *right* data. I learned this the hard way while working on a RNA-seq project. My initial approach was textbook: remove samples with low read counts, high mitochondrial reads, or outliers in PCA plots. But when I compared my filtered results to the raw data, I noticed a pattern: some of the "outliers" were actually the most biologically interesting samples.

So I adjusted. Instead of hard cutoffs, I used dynamic thresholds based on the distribution of my data. For example, I kept samples within 2 standard deviations of the mean for most metrics, but flagged (rather than removed) those just outside the range for manual review. Here’s a snippet of the Python code I used to visualize and decide:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Mock QC metrics: read_count, mitochondrial_reads, etc.
data = pd.DataFrame({
    "sample": [f"S{i}" for i in range(1, 101)],
    "read_count": np.random.lognormal(mean=10, sigma=0.5, size=100),
    "mito_reads": np.random.beta(a=2, b=5, size=100) * 100
})

# Plot read counts
plt.figure(figsize=(10, 4))
plt.subplot(1, 2, 1)
plt.hist(data["read_count"], bins=20, color="skyblue")
plt.axvline(data["read_count"].mean() - 2 * data["read_count"].std(), color="red", linestyle="--")
plt.axvline(data["read_count"].mean() + 2 * data["read_count"].std(), color="red", linestyle="--")
plt.title("Read Count Distribution")

# Plot mitochondrial reads
plt.subplot(1, 2, 2)
plt.hist(data["mito_reads"], bins=20, color="salmon")
plt.axvline(data["mito_reads"].mean() + 2 * data["mito_reads"].std(), color="red", linestyle="--")
plt.title("Mitochondrial Reads %")
plt.tight_layout()
plt.show()
```

The key takeaway? QC filtering should be iterative and adaptive. Don’t just rely on default thresholds—understand your data, visualize it, and let biology guide your decisions. And always, *always* check what you’re throwing away.

What’s the most valuable lesson you’ve learned from QC filtering? Any horror stories (or success stories) from your own data?

#Bioinformatics #DataQuality #RNAseq #Genomics #DataScience
[More on my GitHub](https://github.com/bhavinmoriya)
