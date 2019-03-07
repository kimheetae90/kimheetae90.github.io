---
title: "ClassDiagram"
date: 2019-03-07 14:30:28 +0900
categories: programing
---

## L3DSR 소개

보통 서비스에서는 Inbound traffic 대비 Outbound traffic이 월등히 높은데 Outbound traffic을 SLB에서 모두 수용하게 될 경우 리소스 소모가 커질 수 밖에 없습니다. 그래서 Outbound traffic을 서버가 SLB에 전달하지 않고 직접 클라이언트에게 전달해 SLB의 리소스 소모 방지를 위해 사용하는 구성이 DSR(Direct Server Return) 구성입니다. 그리고 클라이언트의 Request를 서버로 전달함에 있어 어떤 헤더를 이용하는지에 따라 L2/L3 DSR로 구분하게 됩니다. L3DSR 은 Load Balancing 을 위한 메커니즘 중 DSR 방식에서 기존 L2 Layer 의 DSR 방식의 한계를 개선하기 위해 사용하는 기술입니다. L3DSR 구성은 IP 헤더중 어떠한것을 이용하는지에 따라 IP Tunnel 기반과 TOS(DSCP) 기반으로 구분되는데 카카오는 DSCP 기반의 L3DSR 방식을 사용하고 있습니다.

### L2DSR 과 L3DSR의 차이

L2DSR은 L2 Layer 헤더인 MAC 주소 변경을 통해 클라이언트의 Request가 전달되는 반면 L3DSR은 IP헤더를 변조하여 서버에 Request를 전달하는 구성입니다. L2DSR의 경우는 MAC 주소 변경을 위해 서버와 ADC 모두 동일한 Broadcast 도메인에 포함되어야 했고, 그로인한 물리적 회선, 위치등의 한계성이 있었습니다. 그러나 L3DSR의 경우 SLB에서 IP 주소 변경을 통해 클라이언트의 Request가 서버로 전달되기 때문에 L2DSR에서의 물리적인 한계성을 극복 할 수 있습니다. 카카오는 서로 다른곳에 각각 위치한 IDC라도 L3DSR 구성을 통해 Load Balancing이 가능하도록 사용하고 있습니다.