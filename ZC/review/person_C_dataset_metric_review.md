| Dataset | Input Type | Task | Annotation | Metric | Size | Suitable for VLM | Suitable for World Model |
|:-------:|:----------:|:----:|:----------:|:------:|:----:|:----------------:|:------------------------:|
|VQAOnline|问题+上下文+图片|利用“Q+C+I",生成详细真实的长文本回答|Answer Annotation(利用Stack Exchange 社区已经存在的标注结果)<br>User Intention Annotation(每个样本：用户提问意图)<br>Human Evaluation Annotation(用于模型评测)|ROUGE-L<br>METEOR<br>BERTScore<br>CLIP-S<br>RefCLIP-S|64,696|YES|NO|
|VQA v2.0|1图片+1文本问题|根据给定的1张图片和1个文本问题，生成答案|question_id<br>image_id<br>question_type<br>answer_type<br>multiple_choice_answer(从10个答案中选出众数作为标准答案)<br>answers|Accuracy|Train:图片82783 问题443757 标注答案444万<br>Val:图片40504 问题214354 标注答案214万<br>Test:图片81434 问题447793 标注答案-|Yes|No|
