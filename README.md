# ==============================
# Load Libraries
# ==============================
install.packages(c("tidyverse","janitor","skimr","corrplot","caret"))
library(tidyverse)
library(janitor)
library(skimr)
library(corrplot)
library(caret)


# ==============================
# Load Data
# ==============================
df <- read_csv("data/your_dataset.csv")  # Replace with your path

# Clean column names
df <- clean_names(df)

# Remove duplicate rows
df <- df %>% distinct()

# ==============================
# Drop Columns with Too Many Missing Values
# ==============================
# Dynamic drop: > 10 missing values
cols_to_drop <- colnames(df)[colSums(is.na(df)) > 10]
df <- df %>% select(-all_of(cols_to_drop))
cols_to_drop  # Shows which columns were dropped

# ==============================
# Impute Remaining Missing Numeric Values
# ==============================
numeric_cols <- df %>% select(where(is.numeric)) %>% colnames()

for(col in numeric_cols){
  df[[col]][is.na(df[[col]])] <- mean(df[[col]], na.rm = TRUE)
}

# Check if all missing values handled
colSums(is.na(df))

# ==============================
# Basic EDA
# ==============================
# Structure & summary
str(df)
summary(df)

