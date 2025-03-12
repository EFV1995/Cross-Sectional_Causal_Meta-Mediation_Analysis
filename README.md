# Cross-Sectional_Causal_Meta-Mediation_Analysis
This script contains all code include in the original article "Novel Insights into How Pregnancy Diet Affects Maternal-Infant Microbiota: A Cross-Sectional Causal Meta-Mediation Analysis One Month Postpartum".
#Loop mediation with all variables and using all cores (-1)  n = 21

# Load required libraries
library(dplyr)
library(data.table)
library(future)
library(future.apply)
library(writexl)
library(progressr)

# Load mediation function
source("scripts/mediation_analysis.R")

# ✅ Load Data
merged_04_clean <- fread("data/merged_04_clean.csv")  # Ensure the file is in the data folder

# ✅ Enable Multi-core Processing
num_cores <- parallel::detectCores() - 1  # Use all available cores except 1
plan(multisession, workers = num_cores)
cat("Using", num_cores, "cores for parallel processing.\n")

# ✅ Convert data frame to data.table
setDT(merged_04_clean)

# ✅ Generate All Combinations of Variables
combinations <- expand.grid(dietary_indices, maternal_core, infant_core, stringsAsFactors = FALSE)
colnames(combinations) <- c("Diet", "Mediator", "Outcome")

# ✅ Execute Mediation Analysis with Progress Bar
mediation_results <- data.table()
with_progress({
  p <- progressor(steps = nrow(combinations))
  mediation_results <- rbindlist(future_lapply(1:nrow(combinations), function(i) {
    p(message = paste("Processing:", combinations$Diet[i], "->", 
                      combinations$Mediator[i], "->", combinations$Outcome[i]))
    mediation_analysis(combinations$Diet[i], combinations$Mediator[i], combinations$Outcome[i], 
                       data = merged_04_clean, adjust = TRUE)
  }))
})

# ✅ Save Results
write_xlsx(mediation_results, path = "results/mediation_results.xlsx")

# ✅ Clean up
gc()
