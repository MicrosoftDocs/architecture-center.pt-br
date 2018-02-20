---
title: Nivelamento de Carga Baseado em Fila
description: "Use uma fila que funcione como um buffer entre uma tarefa e um serviço que ela invoca para simplificar cargas pesadas intermitentes."
keywords: "padrão de design"
author: dragon119
ms.date: 06/23/2017
pnp.series.title: Cloud Design Patterns
pnp.pattern.categories:
- messaging
- availability
- performance-scalability
- resiliency
ms.openlocfilehash: 99b226511fe14bffdab3cdcf65d4e6cffe89bba6
ms.sourcegitcommit: 8ab30776e0c4cdc16ca0dcc881960e3108ad3e94
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/08/2017
---
# <a name="queue-based-load-leveling-pattern"></a><span data-ttu-id="1829e-104">Padrão de nivelamento de carga baseado em fila</span><span class="sxs-lookup"><span data-stu-id="1829e-104">Queue-Based Load Leveling pattern</span></span>

[!INCLUDE [header](../_includes/header.md)]

<span data-ttu-id="1829e-105">Use uma fila que funcione como um buffer entre uma tarefa e um serviço que ela invoca para simplificar sobrecargas intermitentes que podem causar falha no serviço ou a fazer a tarefa atingir o tempo limite. Isso pode ajudar a minimizar o impacto dos picos de demanda sobre a disponibilidade e a capacidade de resposta para a tarefa e o serviço.</span><span class="sxs-lookup"><span data-stu-id="1829e-105">Use a queue that acts as a buffer between a task and a service it invokes in order to smooth intermittent heavy loads that can cause the service to fail or the task to time out. This can help to minimize the impact of peaks in demand on availability and responsiveness for both the task and the service.</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="1829e-106">Contexto e problema</span><span class="sxs-lookup"><span data-stu-id="1829e-106">Context and problem</span></span>

<span data-ttu-id="1829e-107">Muitas soluções na nuvem envolvem a execução de tarefas que invocam serviços.</span><span class="sxs-lookup"><span data-stu-id="1829e-107">Many solutions in the cloud involve running tasks that invoke services.</span></span> <span data-ttu-id="1829e-108">Nesse ambiente, se um serviço estiver sujeito a cargas pesadas intermitentes, isso poderá causar problemas de desempenho ou confiabilidade.</span><span class="sxs-lookup"><span data-stu-id="1829e-108">In this environment, if a service is subjected to intermittent heavy loads, it can cause performance or reliability issues.</span></span>

<span data-ttu-id="1829e-109">Um serviço pode ser parte da mesma solução que as tarefas que o utilizam ou pode ser um serviço de terceiros fornecendo acesso a recursos usados com frequência, como cache ou serviço de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="1829e-109">A service could be part of the same solution as the tasks that use it, or it could be a third-party service providing access to frequently used resources such as a cache or a storage service.</span></span> <span data-ttu-id="1829e-110">Se o mesmo serviço for usado por várias tarefas em execução ao mesmo tempo, poderá ser difícil prever o volume de solicitações para o serviço a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="1829e-110">If the same service is used by a number of tasks running concurrently, it can be difficult to predict the volume of requests to the service at any time.</span></span>

<span data-ttu-id="1829e-111">Um serviço pode apresentar picos de demanda que causam a sobrecarga, não conseguindo responder às solicitações no tempo adequado.</span><span class="sxs-lookup"><span data-stu-id="1829e-111">A service might experience peaks in demand that cause it to overload and be unable to respond to requests in a timely manner.</span></span> <span data-ttu-id="1829e-112">Inundar um serviço com um grande número de solicitações simultâneas também pode resultar em falha do serviço se não for possível tratar a contenção que essas solicitações causam.</span><span class="sxs-lookup"><span data-stu-id="1829e-112">Flooding a service with a large number of concurrent requests can also result in the service failing if it's unable to handle the contention these requests cause.</span></span>

## <a name="solution"></a><span data-ttu-id="1829e-113">Solução</span><span class="sxs-lookup"><span data-stu-id="1829e-113">Solution</span></span>

<span data-ttu-id="1829e-114">Refatore a solução e introduza uma fila entre a tarefa e o serviço.</span><span class="sxs-lookup"><span data-stu-id="1829e-114">Refactor the solution and introduce a queue between the task and the service.</span></span> <span data-ttu-id="1829e-115">A tarefa e o serviço são executados de maneira assíncrona.</span><span class="sxs-lookup"><span data-stu-id="1829e-115">The task and the service run asynchronously.</span></span> <span data-ttu-id="1829e-116">A tarefa envia uma mensagem que contém os dados exigidos pelo serviço a uma fila.</span><span class="sxs-lookup"><span data-stu-id="1829e-116">The task posts a message containing the data required by the service to a queue.</span></span> <span data-ttu-id="1829e-117">A fila atua como um buffer, armazenando a mensagem até que ela seja recuperada pelo serviço.</span><span class="sxs-lookup"><span data-stu-id="1829e-117">The queue acts as a buffer, storing the message until it's retrieved by the service.</span></span> <span data-ttu-id="1829e-118">O serviço recupera as mensagens da fila e as processa.</span><span class="sxs-lookup"><span data-stu-id="1829e-118">The service retrieves the messages from the queue and processes them.</span></span> <span data-ttu-id="1829e-119">Solicitações de várias tarefas, que podem ser geradas a uma taxa altamente variável, podem ser passadas para o serviço pela mesma fila de mensagens.</span><span class="sxs-lookup"><span data-stu-id="1829e-119">Requests from a number of tasks, which can be generated at a highly variable rate, can be passed to the service through the same message queue.</span></span> <span data-ttu-id="1829e-120">Esta figura mostra o uso de uma fila para nivelar a carga em um serviço.</span><span class="sxs-lookup"><span data-stu-id="1829e-120">This figure shows using a queue to level the load on a service.</span></span>

![Figura 1 – Como usar uma fila para nivelar a carga em um serviço](./_images/queue-based-load-leveling-pattern.png)

<span data-ttu-id="1829e-122">A fila separa as tarefas do serviço e o serviço pode manipular as mensagens em seu próprio ritmo, independentemente do volume de solicitações de tarefas simultâneas.</span><span class="sxs-lookup"><span data-stu-id="1829e-122">The queue decouples the tasks from the service, and the service can handle the messages at its own pace regardless of the volume of requests from concurrent tasks.</span></span> <span data-ttu-id="1829e-123">Além disso, não haverá nenhum atraso para uma tarefa se o serviço não estiver disponível no momento em que ele envia uma mensagem à fila.</span><span class="sxs-lookup"><span data-stu-id="1829e-123">Additionally, there's no delay to a task if the service isn't available at the time it posts a message to the queue.</span></span>

<span data-ttu-id="1829e-124">Esse padrão proporciona os seguintes benefícios:</span><span class="sxs-lookup"><span data-stu-id="1829e-124">This pattern provides the following benefits:</span></span>

- <span data-ttu-id="1829e-125">Pode ajudar a maximizar a disponibilidade porque atrasos decorrentes de serviços não terão um impacto direto e imediato sobre o aplicativo, que pode continuar a postar mensagens na fila mesmo quando o serviço não está disponível ou atualmente não está processando mensagens.</span><span class="sxs-lookup"><span data-stu-id="1829e-125">It can help to maximize availability because delays arising in services won't have an immediate and direct impact on the application, which can continue to post messages to the queue even when the service isn't available or isn't currently processing messages.</span></span>
- <span data-ttu-id="1829e-126">Pode ajudar a maximizar a escalabilidade, pois tanto o número de filas quanto o número de serviços pode ser variado para atender à demanda.</span><span class="sxs-lookup"><span data-stu-id="1829e-126">It can help to maximize scalability because both the number of queues and the number of services can be varied to meet demand.</span></span>
- <span data-ttu-id="1829e-127">Pode ajudar a controlar os custos, pois o número de instâncias de serviço implantadas só precisa ser suficiente para atender a carga média, em vez da carga de pico.</span><span class="sxs-lookup"><span data-stu-id="1829e-127">It can help to control costs because the number of service instances deployed only have to be adequate to meet average load rather than the peak load.</span></span>

    >  <span data-ttu-id="1829e-128">Alguns serviços implementam limitação quando a demanda atinge um limite além do qual o sistema poderia falhar.</span><span class="sxs-lookup"><span data-stu-id="1829e-128">Some services implement throttling when demand reaches a threshold beyond which the system could fail.</span></span> <span data-ttu-id="1829e-129">A limitação pode reduzir a funcionalidade disponível.</span><span class="sxs-lookup"><span data-stu-id="1829e-129">Throttling can reduce the functionality available.</span></span> <span data-ttu-id="1829e-130">Você pode implementar nivelamento de carga com esses serviços para garantir que esse limite não seja atingido.</span><span class="sxs-lookup"><span data-stu-id="1829e-130">You can implement load leveling with these services to ensure that this threshold isn't reached.</span></span>

## <a name="issues-and-considerations"></a><span data-ttu-id="1829e-131">Problemas e considerações</span><span class="sxs-lookup"><span data-stu-id="1829e-131">Issues and considerations</span></span>

<span data-ttu-id="1829e-132">Considere os seguintes pontos ao decidir como implementar esse padrão:</span><span class="sxs-lookup"><span data-stu-id="1829e-132">Consider the following points when deciding how to implement this pattern:</span></span>

- <span data-ttu-id="1829e-133">É necessário implementar lógica de aplicativo que controle a taxa à qual os serviços processam as mensagens para evitar sobrecarregar o recurso de destino.</span><span class="sxs-lookup"><span data-stu-id="1829e-133">It's necessary to implement application logic that controls the rate at which services handle messages to avoid overwhelming the target resource.</span></span> <span data-ttu-id="1829e-134">Evite passar picos de demanda para o próximo estágio do sistema.</span><span class="sxs-lookup"><span data-stu-id="1829e-134">Avoid passing spikes in demand to the next stage of the system.</span></span> <span data-ttu-id="1829e-135">Teste o sistema sob carga para garantir que ele ofereça o nivelamento necessário e ajuste o número de filas e o número de instâncias de serviço que manipulam mensagens para conseguir isso.</span><span class="sxs-lookup"><span data-stu-id="1829e-135">Test the system under load to ensure that it provides the required leveling, and adjust the number of queues and the number of service instances that handle messages to achieve this.</span></span>
- <span data-ttu-id="1829e-136">Filas de mensagens são um mecanismo de comunicação unidirecional.</span><span class="sxs-lookup"><span data-stu-id="1829e-136">Message queues are a one-way communication mechanism.</span></span> <span data-ttu-id="1829e-137">Se uma tarefa espera uma resposta de um serviço, pode ser necessário implementar um mecanismo que o serviço pode usar para enviar uma resposta.</span><span class="sxs-lookup"><span data-stu-id="1829e-137">If a task expects a reply from a service, it might be necessary to implement a mechanism that the service can use to send a response.</span></span> <span data-ttu-id="1829e-138">Para obter mais informações, consulte o [Primer de mensagens assíncronas](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="1829e-138">For more information, see the [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>
- <span data-ttu-id="1829e-139">Tenha cuidado ao aplicar o dimensionamento automático a serviços que estão ouvindo solicitações na fila.</span><span class="sxs-lookup"><span data-stu-id="1829e-139">Be careful if you apply autoscaling to services that are listening for requests on the queue.</span></span> <span data-ttu-id="1829e-140">Isso pode resultar em maior contenção para quaisquer recursos que esses serviços compartilhem e reduzir a eficiência do uso da fila para nivelar a carga.</span><span class="sxs-lookup"><span data-stu-id="1829e-140">This can result in increased contention for any resources that these services share and diminish the effectiveness of using the queue to level the load.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="1829e-141">Quando usar esse padrão</span><span class="sxs-lookup"><span data-stu-id="1829e-141">When to use this pattern</span></span>

<span data-ttu-id="1829e-142">Esse padrão é útil para qualquer aplicativo que use serviços sujeitos a sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="1829e-142">This pattern is useful to any application that uses services that are subject to overloading.</span></span>

<span data-ttu-id="1829e-143">Esse padrão não é útil se o aplicativo esperar uma resposta do serviço com latência mínima.</span><span class="sxs-lookup"><span data-stu-id="1829e-143">This pattern isn't useful if the application expects a response from the service with minimal latency.</span></span>

## <a name="example"></a><span data-ttu-id="1829e-144">Exemplo</span><span class="sxs-lookup"><span data-stu-id="1829e-144">Example</span></span>

<span data-ttu-id="1829e-145">Uma função Web do Microsoft Azure armazena dados usando um serviço de armazenamento separado.</span><span class="sxs-lookup"><span data-stu-id="1829e-145">A Microsoft Azure web role stores data using a separate storage service.</span></span> <span data-ttu-id="1829e-146">Se um grande número de instâncias da função web é executado simultaneamente, é possível que o serviço de armazenamento não consiga responder às solicitações com rapidez suficiente para impedir que essas solicitações atinjam o tempo limite ou falhem.</span><span class="sxs-lookup"><span data-stu-id="1829e-146">If a large number of instances of the web role run concurrently, it's possible that the storage service will be unable to respond to requests quickly enough to prevent these requests from timing out or failing.</span></span> <span data-ttu-id="1829e-147">Esta figura realça um serviço que está sendo sobrecarregado com um grande número de solicitações simultâneas de instâncias de uma função web.</span><span class="sxs-lookup"><span data-stu-id="1829e-147">This figure highlights a service being overwhelmed by a large number of concurrent requests from instances of a web role.</span></span>

![Figura 2 – Um serviço que está sendo sobrecarregado com um grande número de solicitações simultâneas de instâncias de uma função web](./_images/queue-based-load-leveling-overwhelmed.png)


<span data-ttu-id="1829e-149">Para resolver esse problema, use uma fila para nivelar a carga entre as instâncias de função web e o serviço de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="1829e-149">To resolve this, you can use a queue to level the load between the web role instances and the storage service.</span></span> <span data-ttu-id="1829e-150">No entanto, o serviço de armazenamento é projetado para aceitar solicitações síncronas e não pode ser facilmente modificado para ler mensagens e gerenciar a taxa de transferência.</span><span class="sxs-lookup"><span data-stu-id="1829e-150">However, the storage service is designed to accept synchronous requests and can't be easily modified to read messages and manage throughput.</span></span> <span data-ttu-id="1829e-151">Você pode introduzir uma função de trabalho para atuar como um serviço de proxy que recebe as solicitações da fila e as encaminha ao serviço de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="1829e-151">You can introduce a worker role to act as a proxy service that receives requests from the queue and forwards them to the storage service.</span></span> <span data-ttu-id="1829e-152">A lógica do aplicativo na função de trabalho pode controlar a taxa em que ele transmite solicitações ao serviço de armazenamento para impedir que o serviço de armazenamento seja sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="1829e-152">The application logic in the worker role can control the rate at which it passes requests to the storage service to prevent the storage service from being overwhelmed.</span></span> <span data-ttu-id="1829e-153">Esta figura ilustra o uso de uma fila e uma função de trabalho para nivelar a carga entre as instâncias de função web e o serviço.</span><span class="sxs-lookup"><span data-stu-id="1829e-153">This figure illustrates using a queue and a worker role to level the load between instances of the web role and the service.</span></span>

![Figura 3 – Usar uma fila e uma função de trabalho para redistribuir a carga entre as instâncias da função web e o serviço](./_images/queue-based-load-leveling-worker-role.png)

## <a name="related-patterns-and-guidance"></a><span data-ttu-id="1829e-155">Diretrizes e padrões relacionados</span><span class="sxs-lookup"><span data-stu-id="1829e-155">Related patterns and guidance</span></span>

<span data-ttu-id="1829e-156">Os padrões e diretrizes a seguir também podem ser relevantes ao implementar esse padrão:</span><span class="sxs-lookup"><span data-stu-id="1829e-156">The following patterns and guidance might also be relevant when implementing this pattern:</span></span>

- <span data-ttu-id="1829e-157">[Prévia de mensagens assíncronas](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="1829e-157">[Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span> <span data-ttu-id="1829e-158">Filas de mensagens são inerentemente assíncronas.</span><span class="sxs-lookup"><span data-stu-id="1829e-158">Message queues are inherently asynchronous.</span></span> <span data-ttu-id="1829e-159">Talvez seja necessário recriar a lógica do aplicativo em uma tarefa se ela tiver sido adaptada de se comunicar diretamente com um serviço para usar uma fila de mensagens.</span><span class="sxs-lookup"><span data-stu-id="1829e-159">It might be necessary to redesign the application logic in a task if it's adapted from communicating directly with a service to using a message queue.</span></span> <span data-ttu-id="1829e-160">Da mesma forma, talvez seja necessário refatorar um serviço para aceitar solicitações de uma fila de mensagens.</span><span class="sxs-lookup"><span data-stu-id="1829e-160">Similarly, it might be necessary to refactor a service to accept requests from a message queue.</span></span> <span data-ttu-id="1829e-161">Como alternativa, talvez seja possível implementar um serviço de proxy, conforme descrito no exemplo.</span><span class="sxs-lookup"><span data-stu-id="1829e-161">Alternatively, it might be possible to implement a proxy service, as described in the example.</span></span>
- <span data-ttu-id="1829e-162">[Padrão de consumidores concorrentes](competing-consumers.md).</span><span class="sxs-lookup"><span data-stu-id="1829e-162">[Competing Consumers pattern](competing-consumers.md).</span></span> <span data-ttu-id="1829e-163">Talvez seja possível executar várias instâncias de um serviço, cada uma agindo como um consumidor de mensagem da fila nivelamento de carga.</span><span class="sxs-lookup"><span data-stu-id="1829e-163">It might be possible to run multiple instances of a service, each acting as a message consumer from the load-leveling queue.</span></span> <span data-ttu-id="1829e-164">Você pode usar essa abordagem para ajustar a taxa à qual as mensagens são recebidas e passadas para um serviço.</span><span class="sxs-lookup"><span data-stu-id="1829e-164">You can use this approach to adjust the rate at which messages are received and passed to a service.</span></span>
- <span data-ttu-id="1829e-165">[Padrão de limitação](throttling.md).</span><span class="sxs-lookup"><span data-stu-id="1829e-165">[Throttling pattern](throttling.md).</span></span> <span data-ttu-id="1829e-166">Uma maneira simples de implementar a limitação com um serviço é usar o nivelamento de carga baseado em fila e encaminhar todas as solicitações para um serviço por meio de uma fila de mensagens.</span><span class="sxs-lookup"><span data-stu-id="1829e-166">A simple way to implement throttling with a service is to use queue-based load leveling and route all requests to a service through a message queue.</span></span> <span data-ttu-id="1829e-167">O serviço pode processar solicitações a uma taxa que garanta que os recursos exigidos pelo serviço não sejam esgotados e para reduzir a quantidade de contenção que poderia ocorrer.</span><span class="sxs-lookup"><span data-stu-id="1829e-167">The service can process requests at a rate that ensures that resources required by the service aren't exhausted, and to reduce the amount of contention that could occur.</span></span>
- <span data-ttu-id="1829e-168">[Conceitos do serviço Fila](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="1829e-168">[Queue Service Concepts](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span> <span data-ttu-id="1829e-169">Informações sobre como escolher um mecanismo de mensagens e enfileiramento em aplicativos do Azure.</span><span class="sxs-lookup"><span data-stu-id="1829e-169">Information about choosing a messaging and queuing mechanism in Azure applications.</span></span>