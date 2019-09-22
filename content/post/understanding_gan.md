---
date: 2019-07-11
title: Understanding How GAN works
weight: 10
---
# GAN (Generative Adversarial Network)

There are two Models in GAN: 
    - G: Generator and 
    - D: Discriminator

# Train Loss Function

The generator will generate some Fake data to cheat Discriminator, the loss
function is designed such that the probability to succeed to cheat D is high.

While for D, there are two inputs, one is the real data, which D should think
it is Real, while for input any data generated from G (no matter how real when human
will look at) D should always regards it as Fake. The loss function is designed
for above two factors.

# A real example

In following repository:

https://github.com/deNsuh/segan-pytorch

A GAN is implemented to improve the speech quality (to reduce the noise).

The core part for the training is in `model.py`:

Now for the D, there are two input, one is from dataset which is real data, the
expected output would be `1.0` as it is real; Another input is the output from
generator, although generator could generate likely pleasant result it is
actually fake (noisy), so the expected output would be `0.0`.

The loss function is the average of above two losses.

```

        ##### TRAIN D #####
        # TRAIN D to recognize clean audio as clean
        # training batch pass
        outputs = discriminator(batch_pairs_var, ref_batch_var)  # out: [n_batch x 1]
        clean_loss = torch.mean((outputs - 1.0) ** 2)  # L2 loss - we want them all to be 1

        # TRAIN D to recognize generated audio as noisy
        generated_outputs = generator(noisy_batch_var, z)
        disc_in_pair = torch.cat((generated_outputs.detach(), noisy_batch_var), dim=1)
        outputs = discriminator(disc_in_pair, ref_batch_var)
        noisy_loss = torch.mean(outputs ** 2)  # L2 loss - we want them all to be 0
        d_loss = 0.5 * (clean_loss + noisy_loss)
```

After loss is defined, we could update the weights.

```
        # back-propagate and update
        discriminator.zero_grad()
        d_loss.backward()
        d_optimizer.step()  # update parameters
```


Similarly, when training generator, the generated output from G will be feed to
D. The expected output for G would be `1.0` (in contrast to `0.0` for D).

```
        ##### TRAIN G #####
        # TRAIN G so that D recognizes G(z) as real
        z = sample_latent()
        generated_outputs = generator(noisy_batch_var, z)
        gen_noise_pair = torch.cat((generated_outputs, noisy_batch_var), dim=1)
        outputs = discriminator(gen_noise_pair, ref_batch_var)

        g_loss_ = 0.5 * torch.mean((outputs - 1.0) ** 2)
        # L1 loss between generated output and clean sample
        l1_dist = torch.abs(torch.add(generated_outputs, torch.neg(clean_batch_var)))
        g_cond_loss = g_lambda * torch.mean(l1_dist)  # conditional loss
        g_loss = g_loss_ + g_cond_loss

```

Update the weights for G:

```

        # back-propagate and update
        generator.zero_grad()
        g_loss.backward()
        g_optimizer.step()

```

# another example

https://github.com/deNsuh/segan-pytorch
