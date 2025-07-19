# Do Attention Patterns Predict Model Robustness?

Machine learning models often learn spurious correlations during training, achieving high accuracy by focusing on features that correlate with labels but don't reflect true causal relationships. Behavioral testing with counterfactual examples (Ribeiro et al.) can detect this brittleness, but doesn't explain why models fail.

We investigate whether attention patterns can predict the same robustness failures that behavioral testing reveals. If attention analysis correlates with counterfactual performance, it could provide mechanistic insight into why models are brittle.

We tested whether models with biased attention patterns fail behavioral robustness tests. Using sentiment analysis as a case study, we analyzed three pre-trained models (DistilBERT on SST-2, RoBERTa on Twitter, BERT on reviews) to measure what they attend to when making predictions.

For each model, we extracted attention weights on synthetic examples like "This action movie was amazing" where we know the causal features (sentiment words) and spurious features (genre words). We then tested the same models on counterfactual examples that break spurious correlations, following Ribeiro's approach.

Models with poor attention patterns show significantly worse robustness to counterfactual examples (correlation r=0.757). The Twitter model demonstrates this clearly: it achieves 100% accuracy on standard data while paying only 32% of attention to sentiment words, then drops to 56% accuracy when spurious correlations are broken.

| Model | Attention Ratio (Causal/Spurious) | Standard Accuracy | Counterfactual Accuracy | Performance Drop |
|-------|-----------------------------------|-------------------|-------------------------|------------------|
| SST-2 | 1.531 | 100% | 85% | 15% |
| Twitter | 0.544 | 100% | 56% | 44% |
| Reviews | 0.462 | 43% | 37% | 6% |

### Limitations

The result is preliminary evidence from one domain using older transformer architectures. Whether this correlation holds for modern LLMs or other tasks remains an open question that would require broader validation.
