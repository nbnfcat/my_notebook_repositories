# llama.cpp/tools/cli

## 用法

<!-- HELP_START -->

<!-- 重要提示：以下列表由 llama-gen-docs 自动生成；请勿手动修改 -->

### 通用参数

| 参数 | 说明 |
| ---- | ---- |
| `-h, --help, --usage` | 打印用法信息并退出 |
| `--version` | 显示版本和构建信息 |
| `--license` | 显示源代码许可证和依赖项 |
| `-cl, --cache-list` | 显示缓存中的模型列表 |
| `--completion-bash` | 打印可 source 的 llama.cpp bash 补全脚本 |
| `--verbose-prompt` | 在生成前打印详细提示（默认：false） |
| `-t, --threads N` | 生成时使用的 CPU 线程数（默认：-1）<br/>（环境变量：LLAMA_ARG_THREADS） |
| `-tb, --threads-batch N` | 批处理和提示处理时使用的线程数（默认：同 --threads） |
| `-C, --cpu-mask M` | CPU 亲和性掩码：任意长度的十六进制数，与 cpu-range 互补（默认：""） |
| `-Cr, --cpu-range lo-hi` | 用于亲和性设置的 CPU 范围，与 --cpu-mask 互补 |
| `--cpu-strict <0\|1>` | 使用严格的 CPU 放置（默认：0） |
| `--prio N` | 设置进程/线程优先级：low(-1), normal(0), medium(1), high(2), realtime(3)（默认：0） |
| `--poll <0...100>` | 使用轮询等待工作（0 - 无轮询，默认：50） |
| `-Cb, --cpu-mask-batch M` | CPU 亲和性掩码：任意长度的十六进制数，与 cpu-range-batch 互补（默认：同 --cpu-mask） |
| `-Crb, --cpu-range-batch lo-hi` | 用于亲和性设置的 CPU 范围，与 --cpu-mask-batch 互补 |
| `--cpu-strict-batch <0\|1>` | 使用严格的 CPU 放置（默认：同 --cpu-strict） |
| `--prio-batch N` | 设置进程/线程优先级：0-normal, 1-medium, 2-high, 3-realtime（默认：0） |
| `--poll-batch <0\|1>` | 使用轮询等待工作（默认：同 --poll） |
| `-c, --ctx-size N` | 提示上下文大小（默认：0，0 = 从模型加载）<br/>（环境变量：LLAMA_ARG_CTX_SIZE） |
| `-n, --predict, --n-predict N` | 预测的 token 数量（默认：-1，-1 = 无限）<br/>（环境变量：LLAMA_ARG_N_PREDICT） |
| `-b, --batch-size N` | 逻辑最大批量大小（默认：2048）<br/>（环境变量：LLAMA_ARG_BATCH） |
| `-ub, --ubatch-size N` | 物理最大批量大小（默认：512）<br/>（环境变量：LLAMA_ARG_UBATCH） |
| `--keep N` | 从初始提示中保留的 token 数量（默认：0，-1 = 全部） |
| `--swa-full` | 使用完整大小的 SWA 缓存（默认：false）<br/>[更多信息](https://github.com/ggml-org/llama.cpp/pull/13194#issuecomment-2868343055)<br/>（环境变量：LLAMA_ARG_SWA_FULL） |
| `-fa, --flash-attn [on\|off\|auto]` | 设置 Flash Attention 使用（'on', 'off', 或 'auto'，默认：'auto'）<br/>（环境变量：LLAMA_ARG_FLASH_ATTN） |
| `-p, --prompt PROMPT` | 用于开始生成的提示；系统消息请使用 -sys |
| `--perf, --no-perf` | 是否启用内部 libllama 性能计时（默认：false）<br/>（环境变量：LLAMA_ARG_PERF） |
| `-f, --file FNAME` | 包含提示的文件（默认：无） |
| `-bf, --binary-file FNAME` | 包含提示的二进制文件（默认：无） |
| `-e, --escape, --no-escape` | 是否处理转义序列（\n, \r, \t, \', \", \\）（默认：true） |
| `--rope-scaling {none,linear,yarn}` | RoPE 频率缩放方法，除非模型指定，否则默认为 linear<br/>（环境变量：LLAMA_ARG_ROPE_SCALING_TYPE） |
| `--rope-scale N` | RoPE 上下文缩放因子，将上下文扩大 N 倍<br/>（环境变量：LLAMA_ARG_ROPE_SCALE） |
| `--rope-freq-base N` | RoPE 基频，用于 NTK 感知缩放（默认：从模型加载）<br/>（环境变量：LLAMA_ARG_ROPE_FREQ_BASE） |
| `--rope-freq-scale N` | RoPE 频率缩放因子，将上下文扩大 1/N 倍<br/>（环境变量：LLAMA_ARG_ROPE_FREQ_SCALE） |
| `--yarn-orig-ctx N` | YaRN：模型的原始上下文大小（默认：0 = 模型训练上下文大小）<br/>（环境变量：LLAMA_ARG_YARN_ORIG_CTX） |
| `--yarn-ext-factor N` | YaRN：外推混合因子（默认：-1.00，0.0 = 完全内插）<br/>（环境变量：LLAMA_ARG_YARN_EXT_FACTOR） |
| `--yarn-attn-factor N` | YaRN：缩放 sqrt(t) 或注意力幅度（默认：-1.00）<br/>（环境变量：LLAMA_ARG_YARN_ATTN_FACTOR） |
| `--yarn-beta-slow N` | YaRN：高修正维度或 alpha（默认：-1.00）<br/>（环境变量：LLAMA_ARG_YARN_BETA_SLOW） |
| `--yarn-beta-fast N` | YaRN：低修正维度或 beta（默认：-1.00）<br/>（环境变量：LLAMA_ARG_YARN_BETA_FAST） |
| `-kvo, --kv-offload, -nkvo, --no-kv-offload` | 是否启用 KV 缓存卸载（默认：启用）<br/>（环境变量：LLAMA_ARG_KV_OFFLOAD） |
| `--repack, -nr, --no-repack` | 是否启用权重重打包（默认：启用）<br/>（环境变量：LLAMA_ARG_REPACK） |
| `--no-host` | 绕过主机缓冲区，允许使用额外缓冲区<br/>（环境变量：LLAMA_ARG_NO_HOST） |
| `-ctk, --cache-type-k TYPE` | K 的 KV 缓存数据类型<br/>允许的值：f32, f16, bf16, q8_0, q4_0, q4_1, iq4_nl, q5_0, q5_1<br/>（默认：f16）<br/>（环境变量：LLAMA_ARG_CACHE_TYPE_K） |
| `-ctv, --cache-type-v TYPE` | V 的 KV 缓存数据类型<br/>允许的值：f32, f16, bf16, q8_0, q4_0, q4_1, iq4_nl, q5_0, q5_1<br/>（默认：f16）<br/>（环境变量：LLAMA_ARG_CACHE_TYPE_V） |
| `-dt, --defrag-thold N` | KV 缓存碎片整理阈值（已弃用）<br/>（环境变量：LLAMA_ARG_DEFRAG_THOLD） |
| `-np, --parallel N` | 并行解码的序列数（默认：1）<br/>（环境变量：LLAMA_ARG_N_PARALLEL） |
| `--mlock` | 强制系统将模型保留在 RAM 中，而不是交换或压缩<br/>（环境变量：LLAMA_ARG_MLOCK） |
| `--mmap, --no-mmap` | 是否内存映射模型（如果禁用 mmap，加载更慢但如果不使用 mlock 可能减少页面交换）（默认：启用）<br/>（环境变量：LLAMA_ARG_MMAP） |
| `-dio, --direct-io, -ndio, --no-direct-io` | 如果可用则使用 DirectIO（默认：禁用）<br/>（环境变量：LLAMA_ARG_DIO） |
| `--numa TYPE` | 尝试优化，有助于某些 NUMA 系统<br/>- distribute: 在所有节点上均匀分布执行<br/>- isolate: 仅在执行开始所在节点上的 CPU 上生成线程<br/>- numactl: 使用 numactl 提供的 CPU 映射<br/>如果之前没有使用此选项运行，建议在使用前清除系统页缓存<br/>参见 https://github.com/ggml-org/llama.cpp/issues/1437<br/>（环境变量：LLAMA_ARG_NUMA） |
| `-dev, --device <dev1,dev2,..>` | 用于卸载的设备列表，逗号分隔（none = 不卸载）<br/>使用 --list-devices 查看可用设备列表<br/>（环境变量：LLAMA_ARG_DEVICE） |
| `--list-devices` | 打印可用设备列表并退出 |
| `-ot, --override-tensor <张量名模式>=<缓冲区类型>,...` | 覆盖张量缓冲区类型<br/>（环境变量：LLAMA_ARG_OVERRIDE_TENSOR） |
| `-cmoe, --cpu-moe` | 将所有混合专家（MoE）权重保留在 CPU 上<br/>（环境变量：LLAMA_ARG_CPU_MOE） |
| `-ncmoe, --n-cpu-moe N` | 将前 N 层的混合专家（MoE）权重保留在 CPU 上<br/>（环境变量：LLAMA_ARG_N_CPU_MOE） |
| `-ngl, --gpu-layers, --n-gpu-layers N` | 存储在 VRAM 中的最大层数，可以是具体数字、'auto' 或 'all'（默认：auto）<br/>（环境变量：LLAMA_ARG_N_GPU_LAYERS） |
| `-sm, --split-mode {none,layer,row}` | 如何在多个 GPU 间拆分模型，可选值：<br/>- none: 仅使用一个 GPU<br/>- layer（默认）: 跨 GPU 拆分层和 KV<br/>- row: 跨 GPU 拆分行<br/>（环境变量：LLAMA_ARG_SPLIT_MODE） |
| `-ts, --tensor-split N0,N1,N2,...` | 每个 GPU 卸载模型的分数，逗号分隔的比例列表，例如 3,1<br/>（环境变量：LLAMA_ARG_TENSOR_SPLIT） |
| `-mg, --main-gpu INDEX` | 用于模型的 GPU（split-mode = none 时），或用于中间结果和 KV（split-mode = row 时）（默认：0）<br/>（环境变量：LLAMA_ARG_MAIN_GPU） |
| `-fit, --fit [on\|off]` | 是否调整未设置的参数以适应设备内存（'on' 或 'off'，默认：'on'）<br/>（环境变量：LLAMA_ARG_FIT） |
| `-fitt, --fit-target MiB0,MiB1,MiB2,...` | 每个设备的 --fit 目标余量（MiB），逗号分隔列表，单个值广播到所有设备，默认：1024<br/>（环境变量：LLAMA_ARG_FIT_TARGET） |
| `-fitc, --fit-ctx N` | --fit 选项可设置的最小 ctx 大小，默认：4096<br/>（环境变量：LLAMA_ARG_FIT_CTX） |
| `--check-tensors` | 检查模型张量数据是否存在无效值（默认：false） |
| `--override-kv KEY=TYPE:VALUE,...` | 高级选项：按键覆盖模型元数据。要指定多个覆盖，请使用逗号分隔的值。<br/>类型：int, float, bool, str。示例：--override-kv tokenizer.ggml.add_bos_token=bool:false,tokenizer.ggml.add_eos_token=bool:false |
| `--op-offload, --no-op-offload` | 是否将主机张量操作卸载到设备（默认：true） |
| `--lora FNAME` | LoRA 适配器路径（使用逗号分隔值加载多个适配器） |
| `--lora-scaled FNAME:SCALE,...` | 带用户定义缩放的 LoRA 适配器路径（格式：FNAME:SCALE,...）<br/>注意：使用逗号分隔值 |
| `--control-vector FNAME` | 添加控制向量<br/>注意：使用逗号分隔值添加多个控制向量 |
| `--control-vector-scaled FNAME:SCALE,...` | 添加带用户定义缩放 SCALE 的控制向量<br/>注意：使用逗号分隔值（格式：FNAME:SCALE,...） |
| `--control-vector-layer-range START END` | 应用控制向量的层范围，起始和结束包含 |
| `-m, --model FNAME` | 模型路径<br/>（环境变量：LLAMA_ARG_MODEL） |
| `-mu, --model-url MODEL_URL` | 模型下载 URL（默认：未使用）<br/>（环境变量：LLAMA_ARG_MODEL_URL） |
| `-dr, --docker-repo [<仓库>/]<模型>[:量化]` | Docker Hub 模型仓库。仓库可选，默认为 ai/。量化可选，默认为 :latest。<br/>示例：gemma3<br/>（默认：未使用）<br/>（环境变量：LLAMA_ARG_DOCKER_REPO） |
| `-hf, -hfr, --hf-repo <用户>/<模型>[:量化]` | Hugging Face 模型仓库；量化可选，不区分大小写，默认为 Q4_K_M，如果仓库中没有 Q4_K_M 则回退到第一个文件。<br/>如果可用，也会自动下载 mmproj。要禁用，添加 --no-mmproj<br/>示例：ggml-org/GLM-4.7-Flash-GGUF:Q4_K_M<br/>（默认：未使用）<br/>（环境变量：LLAMA_ARG_HF_REPO） |
| `-hfd, -hfrd, --hf-repo-draft <用户>/<模型>[:量化]` | 与 --hf-repo 相同，但用于草稿模型（默认：未使用）<br/>（环境变量：LLAMA_ARG_HFD_REPO） |
| `-hff, --hf-file FILE` | Hugging Face 模型文件。如果指定，将覆盖 --hf-repo 中的量化（默认：未使用）<br/>（环境变量：LLAMA_ARG_HF_FILE） |
| `-hfv, -hfrv, --hf-repo-v <用户>/<模型>[:量化]` | 声码器模型的 Hugging Face 模型仓库（默认：未使用）<br/>（环境变量：LLAMA_ARG_HF_REPO_V） |
| `-hffv, --hf-file-v FILE` | 声码器模型的 Hugging Face 模型文件（默认：未使用）<br/>（环境变量：LLAMA_ARG_HF_FILE_V） |
| `-hft, --hf-token TOKEN` | Hugging Face 访问令牌（默认：来自 HF_TOKEN 环境变量的值）<br/>（环境变量：HF_TOKEN） |
| `--log-disable` | 禁用日志 |
| `--log-file FNAME` | 日志输出到文件<br/>（环境变量：LLAMA_LOG_FILE） |
| `--log-colors [on\|off\|auto]` | 设置彩色日志（'on', 'off', 或 'auto'，默认：'auto'）<br/>'auto' 当输出到终端时启用颜色<br/>（环境变量：LLAMA_LOG_COLORS） |
| `-v, --verbose, --log-verbose` | 将详细级别设置为无限（即记录所有消息，用于调试） |
| `--offline` | 离线模式：强制使用缓存，阻止网络访问<br/>（环境变量：LLAMA_OFFLINE） |
| `-lv, --verbosity, --log-verbosity N` | 设置详细级别阈值。详细程度更高的消息将被忽略。值：<br/> - 0: 通用输出<br/> - 1: 错误<br/> - 2: 警告<br/> - 3: 信息<br/> - 4: 调试<br/>（默认：3）<br/><br/>（环境变量：LLAMA_LOG_VERBOSITY） |
| `--log-prefix` | 在日志消息中启用前缀<br/>（环境变量：LLAMA_LOG_PREFIX） |
| `--log-timestamps` | 在日志消息中启用时间戳<br/>（环境变量：LLAMA_LOG_TIMESTAMPS） |
| `-ctkd, --cache-type-k-draft TYPE` | 草稿模型 K 的 KV 缓存数据类型<br/>允许的值：f32, f16, bf16, q8_0, q4_0, q4_1, iq4_nl, q5_0, q5_1<br/>（默认：f16）<br/>（环境变量：LLAMA_ARG_CACHE_TYPE_K_DRAFT） |
| `-ctvd, --cache-type-v-draft TYPE` | 草稿模型 V 的 KV 缓存数据类型<br/>允许的值：f32, f16, bf16, q8_0, q4_0, q4_1, iq4_nl, q5_0, q5_1<br/>（默认：f16）<br/>（环境变量：LLAMA_ARG_CACHE_TYPE_V_DRAFT） |

### 采样参数

| 参数 | 说明 |
| ---- | ---- |
| `--samplers SAMPLERS` | 将按顺序使用的采样器，以 ';' 分隔<br/>（默认：penalties;dry;top_n_sigma;top_k;typ_p;top_p;min_p;xtc;temperature） |
| `-s, --seed SEED` | RNG 种子（默认：-1，-1 使用随机种子） |
| `--sampler-seq, --sampling-seq SEQUENCE` | 简化采样器序列（默认：edskypmxt） |
| `--ignore-eos` | 忽略流结束 token 并继续生成（隐含 --logit-bias EOS-inf） |
| `--temp, --temperature N` | 温度（默认：0.80） |
| `--top-k N` | top-k 采样（默认：40，0 = 禁用）<br/>（环境变量：LLAMA_ARG_TOP_K） |
| `--top-p N` | top-p 采样（默认：0.95，1.0 = 禁用） |
| `--min-p N` | min-p 采样（默认：0.05，0.0 = 禁用） |
| `--top-nsigma, --top-n-sigma N` | top-n-sigma 采样（默认：-1.00，-1.0 = 禁用） |
| `--xtc-probability N` | xtc 概率（默认：0.00，0.0 = 禁用） |
| `--xtc-threshold N` | xtc 阈值（默认：0.10，1.0 = 禁用） |
| `--typical, --typical-p N` | 局部典型采样，参数 p（默认：1.00，1.0 = 禁用） |
| `--repeat-last-n N` | 最后 n 个 token 用于惩罚（默认：64，0 = 禁用，-1 = ctx_size） |
| `--repeat-penalty N` | 惩罚重复序列 token（默认：1.00，1.0 = 禁用） |
| `--presence-penalty N` | 存在惩罚（默认：0.00，0.0 = 禁用） |
| `--frequency-penalty N` | 频率惩罚（默认：0.00，0.0 = 禁用） |
| `--dry-multiplier N` | 设置 DRY 采样乘数（默认：0.00，0.0 = 禁用） |
| `--dry-base N` | 设置 DRY 采样基值（默认：1.75） |
| `--dry-allowed-length N` | 设置 DRY 采样允许长度（默认：2） |
| `--dry-penalty-last-n N` | 设置最后 n 个 token 的 DRY 惩罚（默认：-1，0 = 禁用，-1 = 上下文大小） |
| `--dry-sequence-breaker STRING` | 为 DRY 采样添加序列中断符，清除默认中断符（'\n', ':', '"', '*'）；使用 "none" 表示不使用任何序列中断符 |
| `--adaptive-target N` | adaptive-p：选择概率接近此值的 token（有效范围 0.0 到 1.0；负数 = 禁用）（默认：-1.00）<br/>[更多信息](https://github.com/ggml-org/llama.cpp/pull/17927) |
| `--adaptive-decay N` | adaptive-p：目标随时间适应的衰减率。值越低反应越快，值越高越稳定。<br/>（有效范围 0.0 到 0.99）（默认：0.90） |
| `--dynatemp-range N` | 动态温度范围（默认：0.00，0.0 = 禁用） |
| `--dynatemp-exp N` | 动态温度指数（默认：1.00） |
| `--mirostat N` | 使用 Mirostat 采样。<br/>如果使用，Top K、Nucleus 和局部典型采样器将被忽略。<br/>（默认：0，0 = 禁用，1 = Mirostat，2 = Mirostat 2.0） |
| `--mirostat-lr N` | Mirostat 学习率，参数 eta（默认：0.10） |
| `--mirostat-ent N` | Mirostat 目标熵，参数 tau（默认：5.00） |
| `-l, --logit-bias TOKEN_ID(+/-)BIAS` | 修改 token 出现在补全中的可能性，<br/>例如 `--logit-bias 15043+1` 增加 token ' Hello' 的可能性，<br/>或 `--logit-bias 15043-1` 降低 token ' Hello' 的可能性 |
| `--grammar GRAMMAR` | BNF 风格语法，用于约束生成（参见 grammars/ 目录中的示例） |
| `--grammar-file FNAME` | 读取语法的文件 |
| `-j, --json-schema SCHEMA` | 用于约束生成的 JSON schema（https://json-schema.org/），例如 `{}` 表示任意 JSON 对象<br/>对于包含外部 $refs 的 schema，请改用 --grammar 结合 example/json_schema_to_grammar.py |
| `-jf, --json-schema-file FILE` | 包含用于约束生成的 JSON schema 的文件（https://json-schema.org/），例如 `{}` 表示任意 JSON 对象<br/>对于包含外部 $refs 的 schema，请改用 --grammar 结合 example/json_schema_to_grammar.py |
| `-bs, --backend-sampling` | 启用后端采样（实验性）（默认：禁用）<br/>（环境变量：LLAMA_ARG_BACKEND_SAMPLING） |

### CLI 专用参数

| 参数 | 说明 |
| ---- | ---- |
| `--display-prompt, --no-display-prompt` | 是否在生成时打印提示（默认：true） |
| `-co, --color [on\|off\|auto]` | 为输出着色，区分提示、用户输入和生成内容（'on', 'off', 或 'auto'，默认：'auto'）<br/>'auto' 当输出到终端时启用颜色 |
| `-ctxcp, --ctx-checkpoints, --swa-checkpoints N` | 每个槽位最大上下文检查点数量（默认：32）[更多信息](https://github.com/ggml-org/llama.cpp/pull/15293)<br/>（环境变量：LLAMA_ARG_CTX_CHECKPOINTS） |
| `-cpent, --checkpoint-every-n-tokens N` | 预填充（处理）期间每 N 个 token 创建一个检查点，-1 表示禁用（默认：8192）<br/>（环境变量：LLAMA_ARG_CHECKPOINT_EVERY_NT） |
| `-cram, --cache-ram N` | 设置最大缓存大小（MiB）（默认：8192，-1 - 无限制，0 - 禁用）[更多信息](https://github.com/ggml-org/llama.cpp/pull/16391)<br/>（环境变量：LLAMA_ARG_CACHE_RAM） |
| `--context-shift, --no-context-shift` | 是否在无限文本生成中使用上下文移位（默认：禁用）<br/>（环境变量：LLAMA_ARG_CONTEXT_SHIFT） |
| `-sys, --system-prompt PROMPT` | 与模型一起使用的系统提示（如果适用，取决于聊天模板） |
| `--show-timings, --no-show-timings` | 是否在每次响应后显示计时信息（默认：true）<br/>（环境变量：LLAMA_ARG_SHOW_TIMINGS） |
| `-sysf, --system-prompt-file FNAME` | 包含系统提示的文件（默认：无） |
| `-r, --reverse-prompt PROMPT` | 在 PROMPT 处停止生成，在交互模式下返回控制权 |
| `-sp, --special` | 启用特殊 token 输出（默认：false） |
| `-cnv, --conversation, -no-cnv, --no-conversation` | 是否以对话模式运行：<br/>- 不打印特殊 token 和后缀/前缀<br/>- 同时启用交互模式<br/>（默认：如果聊天模板可用则自动启用） |
| `-st, --single-turn` | 仅运行单轮对话，完成后退出<br/>如果第一轮通过 --prompt 预定义，则不会交互<br/>（默认：false） |
| `-mli, --multiline-input` | 允许写入或粘贴多行内容，无需每行都以 '\' 结尾 |
| `--warmup, --no-warmup` | 是否执行空运行预热（默认：启用） |
| `-mm, --mmproj FILE` | 多模态投影仪文件路径。参见 tools/mtmd/README.md<br/>注意：如果使用 -hf，此参数可省略<br/>（环境变量：LLAMA_ARG_MMPROJ） |
| `-mmu, --mmproj-url URL` | 多模态投影仪文件 URL。参见 tools/mtmd/README.md<br/>（环境变量：LLAMA_ARG_MMPROJ_URL） |
| `--mmproj-auto, --no-mmproj, --no-mmproj-auto` | 是否使用多模态投影仪文件（如果可用），在使用 -hf 时有用（默认：启用）<br/>（环境变量：LLAMA_ARG_MMPROJ_AUTO） |
| `--mmproj-offload, --no-mmproj-offload` | 是否为多模态投影仪启用 GPU 卸载（默认：启用）<br/>（环境变量：LLAMA_ARG_MMPROJ_OFFLOAD） |
| `--image, --audio FILE` | 图像或音频文件路径，与多模态模型一起使用，使用逗号分隔多个文件 |
| `--image-min-tokens N` | 每个图像最少占用的 token 数，仅用于动态分辨率的视觉模型（默认：从模型读取）<br/>（环境变量：LLAMA_ARG_IMAGE_MIN_TOKENS） |
| `--image-max-tokens N` | 每个图像最多占用的 token 数，仅用于动态分辨率的视觉模型（默认：从模型读取）<br/>（环境变量：LLAMA_ARG_IMAGE_MAX_TOKENS） |
| `-otd, --override-tensor-draft <张量名模式>=<缓冲区类型>,...` | 覆盖草稿模型的张量缓冲区类型 |
| `-cmoed, --cpu-moe-draft` | 将草稿模型的所有混合专家（MoE）权重保留在 CPU 上<br/>（环境变量：LLAMA_ARG_CPU_MOE_DRAFT） |
| `-ncmoed, --n-cpu-moe-draft N` | 将草稿模型的前 N 层混合专家（MoE）权重保留在 CPU 上<br/>（环境变量：LLAMA_ARG_N_CPU_MOE_DRAFT） |
| `--chat-template-kwargs STRING` | 为 JSON 模板解析器设置额外参数，必须是有效的 JSON 对象字符串，例如 '{"key1":"value1","key2":"value2"}'<br/>（环境变量：LLAMA_CHAT_TEMPLATE_KWARGS） |
| `--jinja, --no-jinja` | 是否使用 jinja 模板引擎进行聊天（默认：启用）<br/>（环境变量：LLAMA_ARG_JINJA） |
| `--reasoning-format FORMAT` | 控制是否允许思考标签以及是否从响应中提取，以及返回格式；可选值：<br/>- none: 将思考内容保留在 `message.content` 中未解析<br/>- deepseek: 将思考内容放入 `message.reasoning_content`<br/>- deepseek-legacy: 在 `message.content` 中保留 `<think>` 标签，同时填充 `message.reasoning_content`<br/>（默认：auto）<br/>（环境变量：LLAMA_ARG_THINK） |
| `-rea, --reasoning [on\|off\|auto]` | 在聊天中使用推理/思考（'on', 'off', 或 'auto'，默认：'auto'（从模板检测））<br/>（环境变量：LLAMA_ARG_REASONING） |
| `--reasoning-budget N` | 思考的 token 预算：-1 表示无限制，0 表示立即结束，N>0 表示 token 预算（默认：-1）<br/>（环境变量：LLAMA_ARG_THINK_BUDGET） |
| `--reasoning-budget-message MESSAGE` | 当推理预算耗尽时，在结束思考标签前注入的消息（默认：无）<br/>（环境变量：LLAMA_ARG_THINK_BUDGET_MESSAGE） |
| `--chat-template JINJA_TEMPLATE` | 设置自定义 jinja 聊天模板（默认：从模型元数据中获取模板）<br/>如果指定了 suffix/prefix，模板将被禁用<br/>只接受常用模板（除非在此标志之前设置了 --jinja）：<br/>内置模板列表：<br/>bailing, bailing-think, bailing2, chatglm3, chatglm4, chatml, command-r, deepseek, deepseek-ocr, deepseek2, deepseek3, exaone-moe, exaone3, exaone4, falcon3, gemma, gigachat, glmedge, gpt-oss, granite, grok-2, hunyuan-dense, hunyuan-moe, kimi-k2, llama2, llama2-sys, llama2-sys-bos, llama2-sys-strip, llama3, llama4, megrez, minicpm, mistral-v1, mistral-v3, mistral-v3-tekken, mistral-v7, mistral-v7-tekken, monarch, openchat, orion, pangu-embedded, phi3, phi4, rwkv-world, seed_oss, smolvlm, solar-open, vicuna, vicuna-orca, yandex, zephyr<br/>（环境变量：LLAMA_ARG_CHAT_TEMPLATE） |
| `--chat-template-file JINJA_TEMPLATE_FILE` | 设置自定义 jinja 聊天模板文件（默认：从模型元数据中获取模板）<br/>如果指定了 suffix/prefix，模板将被禁用<br/>只接受常用模板（除非在此标志之前设置了 --jinja）：<br/>内置模板列表：<br/>bailing, bailing-think, bailing2, chatglm3, chatglm4, chatml, command-r, deepseek, deepseek-ocr, deepseek2, deepseek3, exaone-moe, exaone3, exaone4, falcon3, gemma, gigachat, glmedge, gpt-oss, granite, grok-2, hunyuan-dense, hunyuan-moe, kimi-k2, llama2, llama2-sys, llama2-sys-bos, llama2-sys-strip, llama3, llama4, megrez, minicpm, mistral-v1, mistral-v3, mistral-v3-tekken, mistral-v7, mistral-v7-tekken, monarch, openchat, orion, pangu-embedded, phi3, phi4, rwkv-world, seed_oss, smolvlm, solar-open, vicuna, vicuna-orca, yandex, zephyr<br/>（环境变量：LLAMA_ARG_CHAT_TEMPLATE_FILE） |
| `--skip-chat-parsing, --no-skip-chat-parsing` | 强制使用纯内容解析器，即使指定了 Jinja 模板；模型将所有内容（包括推理和/或工具调用）输出到 content 字段（默认：禁用）<br/>（环境变量：LLAMA_ARG_SKIP_CHAT_PARSING） |
| `--simple-io` | 使用基础 IO，以获得更好的子进程和有限控制台兼容性 |
| `--draft, --draft-n, --draft-max N` | 投机解码中草稿的 token 数量（默认：16）<br/>（环境变量：LLAMA_ARG_DRAFT_MAX） |
| `--draft-min, --draft-n-min N` | 投机解码中最少草稿 token 数量（默认：0）<br/>（环境变量：LLAMA_ARG_DRAFT_MIN） |
| `--draft-p-min P` | 最小投机解码概率（贪婪）（默认：0.75）<br/>（环境变量：LLAMA_ARG_DRAFT_P_MIN） |
| `-cd, --ctx-size-draft N` | 草稿模型的提示上下文大小（默认：0，0 = 从模型加载）<br/>（环境变量：LLAMA_ARG_CTX_SIZE_DRAFT） |
| `-devd, --device-draft <dev1,dev2,..>` | 用于卸载草稿模型的设备列表，逗号分隔（none = 不卸载）<br/>使用 --list-devices 查看可用设备列表 |
| `-ngld, --gpu-layers-draft, --n-gpu-layers-draft N` | 草稿模型存储在 VRAM 中的最大层数，可以是具体数字、'auto' 或 'all'（默认：auto）<br/>（环境变量：LLAMA_ARG_N_GPU_LAYERS_DRAFT） |
| `-md, --model-draft FNAME` | 投机解码的草稿模型（默认：未使用）<br/>（环境变量：LLAMA_ARG_MODEL_DRAFT） |
| `--spec-replace TARGET DRAFT` | 如果草稿模型和主模型不兼容，将 TARGET 中的字符串替换为 DRAFT |
| `--gpt-oss-20b-default` | 使用 gpt-oss-20b（注意：可以从互联网下载权重） |
| `--gpt-oss-120b-default` | 使用 gpt-oss-120b（注意：可以从互联网下载权重） |
| `--vision-gemma-4b-default` | 使用 Gemma 3 4B QAT（注意：可以从互联网下载权重） |
| `--vision-gemma-12b-default` | 使用 Gemma 3 12B QAT（注意：可以从互联网下载权重） |

<!-- HELP_END -->