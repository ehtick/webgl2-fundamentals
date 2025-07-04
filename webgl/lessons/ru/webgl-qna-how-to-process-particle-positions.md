Title: Как обрабатывать позиции частиц
Description: Как обрабатывать позиции частиц
TOC: Как обрабатывать позиции частиц

## Вопрос:

Используя сетку 4x4x4 в качестве примера, у меня есть 64 вершины (которые я буду называть частицами), которые начинают с определённых позиций относительно друг друга. Эти 64 частицы будут двигаться в направлениях x, y и z, теряя свои начальные позиции относительно друг друга. Однако каждый цикл новые позиции частиц и скорости должны вычисляться на основе исходных начальных отношений между частицей и её исходными соседями.

Я узнал, что мне нужно использовать текстуры и, следовательно, Framebuffers для этого, и теперь я могу записывать две 3DTextures, которые переключаются, чтобы обеспечить функциональность записи и чтения для выполнения этого. Однако в следующем цикле, когда gl_FragCoord передаётся в фрагментный шейдер с новой позицией частицы (которая может быть переключена с другой частицей, например), я не вижу никакого механизма, с помощью которого исходная координата текстуры, которая содержала информацию о частице, будет записана с текущей информацией частицы. Есть ли какой-то механизм, который я не понимаю, который позволяет движущимся частицам иметь свои данные, хранящиеся в статической сетке (3D текстура), с данными каждой частицы, всегда заполняющими ту же координату, чтобы я мог использовать texelFetch для получения данных частицы, а также данных исходных соседей? Могу ли я изменить gl_FragCoord и иметь вывод пикселя там, где я хочу, или это неизменяемая входная переменная?

Как только я решу эту проблему, я надеюсь затем реализовать Transform Feedback для выполнения фактического движения вершин без сброса текстуры в CPU и извлечения данных позиции и повторной загрузки их в GPU для следующего цикла.

Есть ли какие-либо предложения о том, как отслеживать исходную позицию каждой частицы, исходных соседей и текущую позицию относительно этих исходных соседей, используя текстуры, записанные в Framebuffers?

## Ответ:

Я запутался в вашей путанице 

Вот простая система частиц только на JavaScript. Каждая частица начинается в случайном месте и движется в случайном направлении

{{{example url="../webgl-qna-how-to-process-particle-positions-example-1.html"}}}

Вот та же система частиц, всё ещё в JavaScript, но работающая больше как работает WebGL. Я не знаю, будет ли это более или менее запутанно. Важные моменты заключаются в том, что код, который обновляет позиции частиц, называемый `fragmentShader`, не может выбирать, что он обновляет. Он просто обновляет `gl.outColor`. У него также нет входов, кроме `gl.fragCoord` и `gl.currentProgram.uniforms`. currentParticleState - это массив массивов из 4 значений, где, как и раньше, это был массив объектов со свойством position. particleParameters также просто массив массивов из 4 значений вместо массива объектов со значением velocity. Это для имитации того факта, что это были бы текстуры в реальном WebGL, поэтому любое значение, такое как `position` или `velocity`, теряется.

Код, который фактически рисует частицы, не имеет значения.

{{{example url="../webgl-qna-how-to-process-particle-positions-example-2.html"}}}

Вот тот же код в реальном WebGL

{{{example url="../webgl-qna-how-to-process-particle-positions-example-3.html"}}}



<div class="so">
  <div>Вопрос и процитированные части являются 
    CC BY-SA 4.0 от
    <a data-href="https://stackoverflow.com/users/11399215">billvh</a>
    из
    <a data-href="https://stackoverflow.com/questions/56780278">здесь</a>
  </div>
</div> 