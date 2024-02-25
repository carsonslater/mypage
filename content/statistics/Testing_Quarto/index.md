---
author: Carson Slater
categories:
- Statistics
date: "2024-02-25T00:00:05Z"
draft: false
featured: false
image:
summary: "This is simply a test post."
tags:
- Quarto
- Python
title: Testing with Hugo and RMarkdown
---
## Plotting Complex Functions

For a demonstration of a color aesthetic mapping with complex numbers, see below!




```r
# functions
modulus <- function(z) Re(sqrt(z * Conj(z)))
f <- function(z) (z^2 + 1)/(z^2 - 1)
rad2deg <- function(rad, rotate = 0) ((rad + rotate) * 360/(2*pi)) %% 360

# definitions
# boundaries
L <- -2
U <- 2
# making mesh
domain <- seq(L, U, length.out = 1001)

mesh <- expand.grid(domain, domain) |> 
  mutate("x" = Var1, 
         "y" = Var2) |> 
  select(x, y)

# color mapping components
z <- complex(real = mesh$x, imaginary = mesh$y)

fz <- z |> f()

mod_fz <- fz |> modulus()

# I ended using the second one from the Wikipedia page 
# and I added a constant to shift the color, as well as
# a scale of 100 (a = 0.4 too)

a <- 0.4
ell1 <- (2/pi)*atan(mod_fz) + 65
ell2 <- (mod_fz^a/(mod_fz^a + 1))/10 + 65 
ell3 <- 100*mod_fz^a/(mod_fz^a + 1) + 25 # used this one 

# used saturation of 80
my_colors <- Arg(fz) |>
  rad2deg() |>
  cbind(80, ell3) |> farver::encode_colour(from = "hsl")

df <- cbind(mesh, my_colors) |>
  as.data.frame()
```


```r
# plot replica attempt
df |>
  ggplot(aes(x, y)) +
  geom_raster(aes(fill = my_colors)) + 
  labs(x = "Re(z)", y = "Im(z)") +
  scale_fill_identity() +
  coord_equal(xlim = c(-2,2), ylim = c(-2,2))
```

<embed src="index_files/figure-html/6 plot-1.pdf" width="576" type="application/pdf" />


```r
2L + 2L
```

```
## [1] 4
```

