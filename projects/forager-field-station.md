# forager-field-station

`~/Documents/Forager/forager-field-station` · git `master` · public **HF Gradio Space**

**What it is:** "Forager's Field Station" — photograph a wild plant/mushroom and a small
on-device-class model identifies it *or abstains when unsure*. The Space is the **CPU twin** of
the [[forager-ml]] on-device stack, built for the Build Small Hackathon (Backyard AI track).
Demo video + `@thefullnacho` X thread linked in its README.

**Boundary:** serving/UX + the public face of the model. Runs ONNX exports (`models/*_logits.onnx`)
through `pipeline/infer.py` → `pipeline/convergence.py` (deadly-vetoes-safe arbitration, abstain
gates). It does not train anything — models come from [[forager-ml]].

**What it actually serves (verified 2026-06-23):** `ROUTER = domain_router_v2`;
`EXPERTS = [berry_expert, highvalue_expert, medicinals_expert]` — **3 experts**.

**Edges** ([[ligaments]]): inherits its model stack from [[forager-ml]]; inherits brand framing
from [[brand-thesis]] ("refuses when it isn't sure" is the hackathon's honest-fit angle).

**DIVERGENCE (intentional):** `psychedelics_expert` is omitted here **for optics** on the public
Space, though forager_ml trains it. Decision recorded in [[model-registry]] — keep, don't "fix."
**VERIFY:** serves `domain_router_v2` while forager_ml's published manifest is v1 (likely
unintentional). **VERIFY:** README says experts are ~9M params; forager_ml says ~4.9M — reconcile.
